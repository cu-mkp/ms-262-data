default namespace = ""
namespace a = "http://relaxng.org/ns/compatibility/annotations/1.0"
namespace s = "http://www.ascc.net/xml/schematron"

start =
  
  ## Root element for transcription of folio
  element root {
    attribute page { xsd:NMTOKEN },
    attribute image { xsd:anyURI },
    attribute layout { "three-column" | "four-column" }?,
    mixed {
      (# <text/>
       e.ab
       | e.figure
       | e.mark
       | e.div
       | e.hr)+
    }
  }
#

# BLOCK ELEMENTS

#

# ab

## Anonymous Block: a generic block of text
e.ab =
  element ab {
    a.continued?,
    a.continues?,
    a.margin?,
    a.render?,
    a.cancelled?,
    a.hand?,
    m.phrase
  }
# div

## Text Division: A group of one or more document objects forming a primary textual component; e.g., an "entry" or "recipe"
e.div =
  [
    s:pattern [
      name = "div-id"
      "\x{a}" ~
      "      "
      s:rule [
        context = "div"
        "\x{a}" ~
        "        "
        s:assert [
          test = "@id"
          " \x{a}" ~
          "         Warning: div without an id attribute"
        ]
        "\x{a}" ~
        "      "
      ]
      "\x{a}" ~
      "      "
    ]
  ]
  element div {
    a.continued?,
    a.continues?,
    a.id?,
    a.margin?,
    a.categories?,
    a.part?,
    e.head?,
    (text | e.ab | e.figure | e.hfol | m.phrase)+
  }
# figure

## Figure: a graphical object in the source document
e.figure =
  element figure {
    a.id?,
    a.margin?,
    a.render?,
    a.size?,
    a.alt-text?,
    a.link?,
    (m.phrase
     | element caption { (text | m.phrase)+ })+
  }
# hfol

## hfol: the original foliation of the manuscript in Hebrew numerals.
e.hfol =
  element hfol {
    a.id?,
    a.margin?,
    a.render?,
    a.size?,
    a.alt-text?,
    a.link?,
    (m.phrase
     | element caption { (text | m.phrase)+ })+
  }
# mark

## Mark: a symbol in the source text represented in the transcription by a utf-8 character
e.mark = element mark { xsd:string }
# head

## Heading: a block of text at the beginning of a textual division functioning as the heading or title of that division. TEI Element
e.head = element head { a.margin?, m.phrase }

## Horizontal rule
e.hr = element hr { empty }
#

# TRANSCRIPTION ELEMENTS

#

## Addition: Text added to the document by author, scribe, but *not* by the editor or transcriber
e.add = element add { m.phrase }
# corr

## Correction: A revision introduced into the transcription by the editor or transcriber
e.corr = element corr { m.phrase }
# del

## Deletion: Content deleted in the document by author, scribe, but *not* by the editor or transcriber
e.del = element del { m.phrase }
# exp

## Expansion: text added by the editor or transcriber expanding an abbreviation in the source
e.exp = element exp { m.phrase }
# gap

a:documentation [
  "Gap: Text added to the document by author, scribe, but *not* by the editor or transcriber"
]
e.gap = element gap { empty }
# ill

## Illegible: unreadable text in the source document
e.ill = element ill { m.phrase }
# incipit

## Incipit: the first few words of an entry, functioning as a heading.  TEI (adapted)
e.incipit = element incipit { m.phrase }
# lb

## Line Break: An empty element indicating end of a line of text in the source document
e.lb = element lb { empty }
# sup

## Supplied Text: Text added by the transcriber/editor.
e.sup = element sup { m.phrase }
# underline

## Underlined: underlined text
e.underline = element underline { m.phrase }
# emph

## Emph: emphasized text
e.emph = element emph { m.phrase }
# superscript

## superscript: superscript text
e.superscript = element superscript { m.phrase }
# ups

## ups: text under paper strip
e.ups = element ups { m.phrase }
# voc

## Voc: text with vowels (nikud)
e.voc = element voc { m.phrase }
# chk

## Chk: text with a chipchuk/geresh (Hebrew symbol that generally indicates abbreviation or foreign words).
e.chk = element chk { m.phrase }
#

# ATTRIBUTES

#

# id

## Identifier: A unique identifier for its parent element 
a.id = attribute id { xsd:NMTOKEN }
# margin

## Margin (position): the location in the margin at which the parent element appears in the source document. Valid values are: "right-top", "right-middle", "right-bottom", "left-top", "left-middle", "left-bottom", "top", "bottom", and "full"
a.margin =
  attribute margin {
    ("left-top"
     | "left-middle"
     | "left-bottom"
     | "right-top"
     | "right-middle"
     | "right-bottom"
     | "top"
     | "bottom"
     | "full")?
  }
# render

## Margin rendition: Instructions to cue the proper rendition of the height and width  a margin block. Valid values are: "tall", "wide", and "full"
a.render = attribute render { ("tall" | "wide" | "full")? }
# size

## Image size: Instructions to cue the proper rendition of the size of an image. Valid values are: "x-small", "small", "medium", and "large"
a.size = attribute size { ("x-small" | "small" | "medium" | "large")? }
# alt-text

## Alt-text: a textual description of a figure; analogous to HTML alt attribute
a.alt-text = attribute alt-text { text }
# link

## Link: the url of a linked image for a figure
a.link = attribute link { xsd:anyURI }
# continues

## cont (continues): An attribute indicating the parent block continues from another page 
a.continues = attribute continues { "yes" }
# continued

## cont (continued): An attribute indicating that the parent block is continued on another page
a.continued = attribute continued { "yes" }
# categories

## Categories: A semi-colon delimited list of controlled terms assigned by the project to a text division
a.categories = attribute categories { xsd:token }
# part

## Part: Indicates that a div is a part of the same text division which shares the same id on the same folio page. Values are "y" and "n".
a.part = attribute part { "y" | "n" }
# cancelled

## Cancelled: Indicates that an ab has been stricken out (or cancelled) in the original. Values are "y" and "n".
a.cancelled = attribute cancelled { "y" | "n" }
# hand

## Hand: Indicates that an ab has been written by a hand other than the main hand of the author-practitioner, hand a. Valid values are: "handa'", "handb", "handc", "handd", and "hande"
a.hand =
  attribute hand { ("handa'" | "handb" | "handc" | "handd" | "hande")? }
#

# LANGUAGE ELEMENTS

#

# owy

## Old Western Yiddish: Germanic language in Hebrew characters
e.owy = element owy { m.phrase }
# heb

## Hebrew: Hebrew text
e.heb = element heb { m.phrase }
# jit

## Judeo-Italian: Italian vernacular transcribed in Hebrew characters
e.jit = element jit { m.phrase }
# lad

## Ladino: Spanish text in Hebrew characters
e.lad = element lad { m.phrase }
# lat

## Latin: Latin text transcribed in Hebrew characters
e.lat = element lat { m.phrase }
# laz

## La'az: Unidentified Romance language transcribed in Hebrew characters
e.laz = element laz { m.phrase }
#

# SEMANTIC ELEMENTS

#

# al

## Animal
e.al = element al { m.phrase }
# bp

## Body Part
e.bp = element bp { m.phrase }
# pro

## Profession
e.pro = element pro { m.phrase }
# ms

## Measurement
e.ms = element ms { m.phrase }
# env

## Environment: Reference to a physical space or environment, such as workshop space, mountains, forest, etc.
e.env = element env { m.phrase }
# m

## Material
e.m = element m { m.phrase }
# pa

## Plant
e.pa = element pa { m.phrase }
# tmp

## Temporal Term: A reference to time, e.g., hour, day, season, holiday, span of time, time of day, etc
e.tmp = element tmp { m.phrase }
# tl

## Tool
e.tl = element tl { m.phrase }
# sn

## Sense Term: Use of the 5 senses to make qualitative assessment, but generally not visual, e.g., “luisant” 
e.sn = element sn { m.phrase }
# pl

## Place Term
e.pl = element pl { m.phrase }
# pn

## Personal Name
e.pn = element pn { m.phrase }
# mn

## Magical Name
e.mn = element mn { m.phrase }
# in

## Incantation
e.in = element in { m.phrase }
# ps

## Psalm
e.ps = element ps { m.phrase }
# cn

## Currency Term: Coins and currency (only type of coin, not amount)
e.cn = element cn { m.phrase }
# mu

## Musical Instrument
e.mu = element mu { m.phrase }
# md

## Medical Term: medical and health-related terms or phrases, such as disease terms, as plague and mange, or medical advice, if possible
e.md = element md { m.phrase }
# wp

## Weapon term: terms related to weapons and weaponry
e.wp = element wp { m.phrase }
# df

## Definition:
e.df = element df { m.phrase }
# comment
e.comment =
  element comment {
    attribute rid { xsd:string }
  }
# PHRASE MODEL

# m.phrase
m.phrase =
  (text
   | e.ab
   | e.add
   | e.al
   | e.bp
   | e.cn
   | e.del
   | e.df
   | e.env
   | e.figure
   | e.hfol
   | e.mark
   | e.heb
   | e.jit
   | e.ill
   | e.in
   | e.lad
   | e.lat
   | e.laz
   | e.owy
   | e.lb
   | e.m
   | e.mn
   | e.ms
   | e.pa
   | e.pl
   | e.pn
   | e.pro
   | e.ps
   | e.sn
   | e.sup
   | e.tl
   | e.tmp
   | e.wp
   | e.corr
   | e.gap
   | e.incipit
   | e.md
   | e.mu
   | e.underline
   | e.superscript
   | e.exp
   | e.comment
   | e.hr
   | e.emph
   | e.ups
   | e.voc
   | e.chk)+
