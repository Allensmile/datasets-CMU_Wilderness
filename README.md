# datasets-CMU_Wilderness
CMU Wilderness Multilingual Speech Dataset

Under construction

A dataset of over 700 different languages providing audio, aligned
text and word pronunciations.  On average each language provides
around 20 hours of sentence-lengthed transcriptions.  Data is mined
from New Testaments from http://www.bible.is/

List of Languages with realtive scores of accuracy of alignment

http://festvox.org/cmu_wilderness/

Map of Languages geopistioned

http://festvox.org/cmu_wilderness/map.html

# Language List

The file LangList.txt has a list of all languages with features as space
seperated fields

    1  LANGID six letter language id from bible.is
    2  TLC    three letter language code (iso 639-2)
    3  WIKI   Wikipedia link to language description
    4  START  start url at bible.is
    5  LAT    geo located latitude
    6  LONG   geo located longitude
    7  #utt0  Number utterances found in Pass 0 (cross-lingual alignment)
    8  MCD0   Mel Cepstral Distortion score for Pass 0 (smaller is better)
    9  #utt1  Number utterances found in Pass 1 (in-language alignment)
    10 MCD1   Mel Cepstral Distortion score for Pass 1 (smaller is better)
    11 Dur    HH:MM:SS duration of alignment data (from Pass 1)
    12 MCDB   Mel Cepstral Distortion score for base CG synthesizer
    13 MCDR   Mel Cepstral Distortion score for Random Forest CG synthesizer
    14- NAME  Text name of language (may be multiple fields)
    
# Prerequisites

Ubuntu (and related) prerequisites:

    sudo apt-get install git build-essential libncurses5-dev sox
    sudo apt-get install csh ffmpeg html2text

# Make Dependencies

Builds the FestVox voice building tools in build/ and sets up the
environment variable settings in festvox_env_settings

    ./bin/do_found make_dependencies

# Create Alignments For A Language

Because we cannot redistribution the audio from bible.is, you must
download that diat directly, then build the alignments using the
idices we distribute.

Alignments (short waveforms plus transcripts) may be recreated for
a language from the packed versions in the indices/ directory.  You
need to know the six letter code for the languages (see LangList for
mappings).  In this example we use NANTTV (Hokkien) to illustrate the
commands, but you should substitute the code for your desired language.

    nohup ./bin/do_found fast_make_align indices/NANTTV.tar.gz &

This will unpack the indices in the NANTTV directory, download the
data from bible.is (unless it is already in
downloads/NANTTV/download/) then reconstruct the aligned data in
NANTTV/aligned/wav and NANTTV/aligned/etc This process will take
around 30 minutes depending on your internet connection, and the
speech of your machine.

...

# Create Text To Speech Model

Given the alignments in aligned/ you can build a speech synthesizer
for Festival (and Flite) as follows.

    cd NANTTV
    nohup ../bin/do_found make_tts &
    ../bin/do_found get_voices

While build a Random Forest Clustergen synthesis model for Festival
and Flite in NANTTV/voices/ This will take at least 48 hours on a 12
core machine.

# Create Speech To Text Model

    cd NANTTV
    nohup ../bin/do_found make_asr &

This does not (yet) build a model, but gives a punctuation free transcription
file in NANTTV/aligned/etc/transcription_nopunct.txt and a pronunciation
lexicon in NANTTV/aligned/etc/pronunciation_lex

# Creating New Alignments

You can do the full allignment creation if you want.  Our alignments
certainly can be improved on with better acoustic models,
pronunciations etc.  If you are interested in re-aligning you can do so
with the commant

    nohup ./bin/do_found full_make_align http://listen.bible.is/NANTTV/Matt/1/D &

This may take around 7 days on a 12 core machine.  It needs about 150GB of
diskspace (which can be reduce with the command ../bin/do_found tidy_up at the
end to about 20GB).  The alignments themselves are usually 2GB.

Aligning all 700 languages will take around 13 years on a single machine.

# Acknowledgements

This dataset was prepared by Alan W Black (awb@cs.cmu.edu) with substantial
help from a large number of CMU students.  We also would like to thank
various members of the CMU community, especially Florian Metze, for access
to CPU resources to help calculate the alignments.  This work was in part
funded by the DARPA Lorelai Program.







