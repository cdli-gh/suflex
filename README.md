# FLEx prototype for Sumerian (experimental)

FLEx provides shallow, dictionary-based morphological analysis similar to the ORACC Lemmatizer.
It is designed for creating dictionaries. It is widely used in language documentation and
linguistic typology, but normally, FLEx data is not shared, but only the resulting dictionary.
No guarantees for portability. We use FieldWorks version 9.0.

The sample data is the glosses from Jagersma, Chap. 5 (using his notations and glosses)

## Configuration (FLEx)

See http://packages.sil.org/ for the installation of FLEx. Tested for Ubuntu 20.04 LTS.

    $> (wget -O- https://packages.sil.org/keys/pso-keyring-2016.gpg | sudo tee /etc/apt/trusted.gpg.d/pso-keyring-2016.gpg)&>/dev/null
    $> (. /etc/os-release && sudo tee /etc/apt/sources.list.d/packages-sil-org.list>/dev/null <<< "deb http://packages.sil.org/$ID $VERSION_CODENAME main")
    $> sudo apt update
    $> sudo apt install fieldworks

Then, log out and in

Run FLEx with

    $> fieldworks-flex &

In the GUI, open FILE:Project Management:Project Locations and set to the parent directory of this one.

(Alternatively, try to import the project, in particular in case of version conflicts, but according to rumors, that's not guaranteed to be complete.)

## Idiosyncrasies

FLEx does not support character splitting with -, but instead, considers this a word separator. There is no way to override this behavior. There does not seem to be any way to re-define punctuation characters.

For writing transcriptions, I used the following (reversible) heuristics:

- by default write lower case only, write subscripts as plain numbers
- camel case instead of -, e.g., `ninAnNa` for `nin-an-na` (omit all -, write following character in upper case)
- mark the beginning of determiners with `²` (this is an arbitrary symbol, but not split off, at least), apply the upper case rule to the next character (if the determiner is preceded by original `-`), if a determiner precedes a character, write it in upper case, so write `²dEnKi` for `{d}en-ki`
- FLEx does not allow to use "standard" Assyriological morpheme segmenters, but requires to use `-` in place of the CDLI symbol `=` (`=` does exist in FLEx, too, but has a different meaning)

## Why not

CON:
- idiosyncratic notations
- quite mouse-oriented, sometimes slow for certain annotations, but there are keyboard shortcuts that can help
- lexicon hard to maintain, no corrections from glossing mode, need to switch to lexicon tab first, then find the right entry
- slow, changing tabs takes up to a second even for tiny data (a single 3 word gloss, 2 dictionary entries)
- for fixing typos in the spelling, this cannot be done from the `Analyze` tab, but you need to switch to `Baseline` tab before
- convoluted menus: there are two systems of tabs: resource tabs (low left) and operations within the resource pane (top right)
- not super stable, esp. on Linux. Can freeze
- creates dependencies from a specific (and occasionally, cryptic) 20-year piece of software with poorly documented exchange formats
- very small (if even existing) developer community
- technology largely out of sync with current NLP
- not compatible with related tools, esp., Toolbox (created by the same people for the same purpose, just 15 years earlier)
- semi-automated, dictionary-based annotation requires manuel disambiguation
- format is cryptic and hard to process

PRO:
- we could try to automatically transform CDLI corpora and bootstrap a lexical analyzer in the process
- support slot grammars (but not obvious how to define slots)
- seems to perform silent backup at every step (but this is also a small CON, because it can actually lead to a freeze. Not sure what happens with an incomplete backup along the way)
- underlying software has been open-sourced
- large and active user community
- more compatible with related work in linguistics
- semiautomated, dictionary-based annotation
- captures all necessary dimensions for Sumerian morphology

## Conclusion

This is a possibility for a linguist (but maybe not a lexicographer) to create baseline resources for morphological annotation and dictionary. The main use would be to let non-techies do some annotations, and it's relatively fast and supports automated annotation.

This is not an option for a proper CDLI workflow. Instead, we would rip off resources created with FLEx for our own processing.
