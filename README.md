
   # Lyrics-Conditioned Neural Melody Generation
   
Feb.14, 2020: Releasing Codes of Condtional LSTM-GAN for Melody Generation from Lyrics at https://drive.google.com/file/d/1j0qhd0YkTp1-q6FNEE7y4O8JpA865KQ5/view?usp=sharing
-------------------------------------------------------------------------------------------------------------------
If you use our lyrics-melody dataset and lyrics embedding (including our skip-gram mdoel and BERT model repectively trained in our lyrics dataset), please kindly cite our paper
"Conditional LSTM-GAN for Melody Generation from Lyrics" available at https://arxiv.org/pdf/1908.05551.pdf (more details about lyrics-melody dataset and melody generation from lyrics)

You can find our 12 melodies (melodies_experiment.zip) used in subjective evaluation of this paper. 
These 12 melodies respectively are generated by baseline method, LSTM-GAN, and ground truth.

--baseline method: bas1-4--; --LSTM-GAN:gen1-4--; --ground truth:GT1-4--

If you have any questions, please let us know. Contact Yi Yu ( http://research.nii.ac.jp/~yiyu/ ) by yiyu@nii.ac.jp and Simon Canales by simon.canales@epfl.ch
--------------------------------------------------------------------------------------------------------
 Summary of answers for Conditional LSTM-GAN for Melody Generation from Lyrics  available at https://arxiv.org/pdf/1908.05551.pdf

Some questions from emails written by the readers are summarized as follows (have merged same/similar questions to avoid unnecessary repetition). We will keep updating answer lists once new questions come out.

Q1 you analyse a dataset of 12,197 MIDI songs and demonstrate the results in Figure 5. I have a question about rest duration and it would be great if you could help me to figure out. It shows that almost 80% of sequences do not have a rest. Does it calculate the breathe break for singers?-------------------- A1: Fig. 5 shows the flattened distribution of the rest duration over all the notes from our dataset. This doesn't mean that 80% of sequences do not have a rest, but that 80% of the notes don't have a rest before them. The rest duration value was calculated according to the note-off, note-on information of MIDI files, using eq. (12) and the allowed rest duration values shown in table 3. You can notice that the shortest rest representation we use is a quarter rest. This means that short silences will be represented with a 0 rest value.

Q2 The Neural Melody Composition from Lyrics paper recommends setting every midi file to the same tempo and key. Would it help to do this with your data? (Or maybe they're the same already? Your paper doesn't say.)---------------------
A2: The data is not set to the same key. However, people are free to train the model with another dataset. The tempo information is used to compute the value of the rest duration or note duration. The tempo is then not taken into account.

Q3 The tempo information is used to compute the value of the rest duration or note duration. The tempo is then not taken into account. What you mean "The tempo is then not taken >into account"?---------------------
A3: The durations’ unit is the beat. Therefore, the absolute duration of a note or a rest is depending on the tempo. However, this is not used during training or sampling. To recreate midi files, we can choose any tempo.

Q4 In I – Introduction 
Existing works, e.g., Markov models [7], random forests [8], and recurrent neural network (RNN)[9], can generate lyrics-conditioned music melody. However, these methods cannot ensure that the distribution of generated data is consistent with that of real samples. Could you detail why?--------------------
A4: Markov models, random forests, and RNN, all can learn the transition probability between adjacent notes in sequences, but they do not explicitly model the distribution of the note sequence. In our work, LSTM does the similar work as RNN. But we adopt the GAN model by adding a discriminator. GAN helps to model unknown distributions, and ensures that the learned distribution (of note sequence) is consistent with that of the real samples.

Q5 In the case of Markov, etc., I understand that the generation may be not very conformant to the style learnt, unless using a high order Markov but with the risk of recopying entire sequences from  the corpus and thus plagiat. But, in the case of a RNN-based architecture [9], what is the rationale?--------------------
A5: As mentioned before, RNN does the similar work as LSTM in our work. But without including a discriminator, it only learns transmission probability between adjacent notes, but does not promise that generated sequences look like real ones.

Q6 I mean you could use a RNN (LSTM) architecture with conditioning. Have you tried to compare your LSTM-GAN-conditioning architecture to a LSTM-GAN equivalent architecture?--------------------
A6: This was tried, and we noticed mode collapse, meaning that there was less variety in the generated melodies. In other words, conditioning seem to reduce mode collapse.

Q7 RNN GAN architecture. How do you use the Generator? Do you enter a sequence of syllables and generate the corresponding sequence of MIDI triplets? Or do you do it iteratively, i.e., generating one by one successive triplets from successive syllables? Please confirm !
--------------------A7: We feed the all sequence to an LSTM, assuring that the output state of the LSTM at time t-1 is given as an input to the same LSTM at time t. This can be viewed as a non-iterative process (unrolled representation of LSTM) or as an equivalent iterative process. In the tensorflow implementation this process is non-iterative.

Q8 The same for the Discriminator, what is the exact input? A sequence of triplets I guess?--------------------
A8: The exact input of discriminator is the generated sequences of MIDI triplets which are the output of generator.

Q9 Baseline model Do you create melodies by concatenating randomly sampled MIDI triplets from the validation set? But the resulting sequence is not significative.--------------------
A9: Melodies from baseline model are created by randomly sampling the testing set based on the dataset distribution of music attributes. The MIDI numbers are restricted between 60 and 80.

--------------------------------------------------------------------------------------------------------
Answers to your questions:

1. --Which folder/file I can use to crawl/have 12,197 MIDI files (7,998+ 4,199) ? -- lmd-full_and_reddit_MIDI_dataset.

2. --Which folder/file I can use to crawl/have 7,998 MIDI ﬁles come from "LMD-full" MIDI Dataset? -- lmd-full_MIDI_dataset.

3. --Which folder/file I can use to craw/have 4,199 MIDI ﬁles come from reddit MIDI dataset? -- The reddit dataset is not parsed alone. You can find it by using "lmd-full_and_reddit_MIDI_dataset".

4. --Which folder/file I can use to get all syllable-level, word-level, and sentence-level embedding vectors extracted from our trained  Skip-gram model? -- Skip-gram_lyrics_encoders (lyric_encoders) contains all trained models.

5. --Which folder/file I can use to get all syllable-level, word-level, and sentence-level embedding vectors extracted from our trained BURT model? -- This is not uploaded due to limited space, but, you can email us to obtain.

6. --Which folder/file I can use to get information about how to directly use the trained Skip-gram model to extract lyrics embedding vectors? -- Skip-gram_model_script_to_extract_syllables_and_word_level_embeddings.ipynb can be used to directly get embeddings.

7. --Which folder/file I can use to get information about how to directly use the trained BURT model to extract lyrics embedding vectors? -- This is not uploaded due to limited space, but, you can email us to obtain.

8. --Which folder/file we can use to get lyrics embedding vectors used in the paper "Conditional GAN-LSTM for Melody Generation from Lyrics"? -- Same script can be used to get the embeddings (Skip-gram_model_script_to_extract_syllables_and_word_level_embeddings.ipynb  can be used to directly to get embeddings).

---------------------------------------------------------------------------------------------------------------------------
More details:

For the project, two differents dataset were used : 
- One dataset that can be found in the "partial_dataset" folder and comes from the LAKH Midi Dataset lmd-full (downloadable at this url : https://colinraffel.com/projects/lmd/). Only English songs were used from the dataset.
  To download the MIDI files corresponding the .npy files from the dataset, you can search the names of the files in both dataset, that   are unchanged and serve as ID.
  This dataset was used for training the LSTM-GAN model. Both word-level parsing and syllable-level parsing were used in the training (see below for more information)
  
  
- One dataset that is made by mergind the one from LAKH Midi Dataset and one found on https://www.reddit.com/r/datasets/comments/3akhxy/the_largest_midi_collection_on_the_internet/. 
  This dataset was used for training the Skip-gram embeddings as well as the BURT embeddings. Fom this dataset, only Word-level parsing was used.


lyrics embeddings for "lmd-full + reddit" are used for training skip-gram model and BURT model, while, lyrics embeddings for "lmd-full" are used for training, validation, and testing in Conditional LSTM-GAN model for melody generation from lyrics. 


The parsing is as follow :

— The syllable parsing :
  This format is the lowest level that pair together every notesand the corresponding syllable and it’s attributes.

— The word parsing :
  This format regroups every notes of a word and gives the attributesof every syllables that makes the word.


— The Sentence parsing :
  Similarly, this format put together every notes that forms asyllable (or in most case, a lyric line) and it’s corresponding attributes.


— The Sentence and word parsing :
  Using the two last mentionned format, this one consist ofparsing the lyrics and notes in sentences and, whithin these sentences, to separateeverything in words.1
  
One note always containing one and only one syllable.
We parsed every songs in continous attributes and discrete attributes.

The discrete attributes are, in order:

— The Pitch of the note :
  In music, the pitch is what decide of how the note should beplayed. We used the Midi note number as an unit of pitch, it can take any integervalue between 0 and 127.

— The duration of the note :
  The duration of the note in number of staves. It can be a quarter note, a half note, a whole note or more. The exhaustive set of values itcan take in our parsing is : [0.25 ;0.5 ;1 ;2 ;4 ;8 ;16 ;32].


— The duration of the rest before the note :
  This value can take the same numerical values as the Duration but it can also be null (so zero).


The continuous attributes are, in order:

— The start of the note : In seconds since the beginning of the sung song.

— The length of the note : In seconds.

— The frequency of the note : In Hertz.

— The velocity of the note : Mesured as an integer by the pretty_midi python package.



An example on the song Listen to the Rhythm of the falling rain in the syllable parsing is :

List    [74.0, 0.5, 0.0]		[0.0, 0.18595050000000057, 587.3295358348151, 110.0]

en  		[72.0, 1.0, 0.0]		[0.247933999999999, 0.24380176666666742, 523.2511306011972, 110.0]

to	  	[72.0, 0.5, 0.0]		[0.24793400000000076, 0.18595050000000057, 523.2511306011972, 110.0]

the	  	[69.0, 1.0, 0.0]		[0.24793400000000076, 0.24380176666666564, 440.0, 110.0]

rhy	  	[69.0, 0.5, 0.0]		[0.247933999999999, 0.18595050000000057, 440.0, 110.0]

thm	  	[67.0, 1.0, 0.0]		[0.24793400000000076, 0.24380176666666564, 391.99543598174927, 110.0]

of		  [67.0, 1.0, 0.0]		[0.247933999999999, 0.24380176666666742, 391.99543598174927, 110.0]

the		  [65.0, 0.5, 0.0]		[0.24793400000000076, 0.18595050000000057, 349.2282314330039, 110.0]

fall		[67.0, 2.5, 0.0]		[0.24793400000000076, 0.41322333333333283, 391.99543598174927, 110.0]

ing		  [65.0, 1.0, 0.0]		[0.49586799999999975, 0.24380176666666564, 349.2282314330039, 110.0]

rain		[65.0, 4.0, 0.0]		[0.247933999999999, 0.9917360000000013, 349.2282314330039, 110.0]

Tel		  [74.0, 1.0, 1.0]		[1.2396700000000003, 0.24380176666666742, 587.3295358348151, 110.0]

ling		[72.0, 0.5, 0.0]		[0.24793400000000076, 0.18595050000000057, 523.2511306011972, 110.0]

me		  [72.0, 1.0, 0.0]		[0.247933999999999, 0.24380176666666742, 523.2511306011972, 110.0]

just		[69.0, 0.5, 0.0]		[0.24793400000000076, 0.18595050000000057, 440.0, 110.0]

what		[69.0, 0.5, 0.0]		[0.24793400000000076, 0.1239669999999986, 440.0, 110.0]

a		    [67.0, 0.5, 0.0]		[0.1239669999999986, 0.12396700000000038, 391.99543598174927, 110.0]

fool		[69.0, 1.5, 0.0]		[0.12396700000000038, 0.37190100000000115, 440.0, 110.0]

I've		[72.0, 0.5, 0.0]		[0.49586799999999975, 0.18595050000000057, 523.2511306011972, 110.0]

been		[72.0, 2.0, 0.0]		[0.24793400000000076, 0.49586799999999975, 523.2511306011972, 110.0]


MIT License

Copyright (c) 2019 Source@Lyrics-Conditioned Neural Melody Generation (tentative)


(We will update our source timely and codes used for this project will be shared soon. Accordingly, license will be updated. )
Permission is hereby granted, free of charge, to any person obtaining a copy
of this source and associated documentation files (the "dataset and embedding vectors"), without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the source.

THE SOURCE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOURCE OR THE USE OR OTHER DEALINGS IN THE
SOURCE.

