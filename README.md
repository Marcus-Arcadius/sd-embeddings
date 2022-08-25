# sd-embeddings
A place to share fine-tuned embeddings for stable-diffusion, and to discuss the most user-friendly way to create and use them with merged embeddings in mind.

Some notes (thanks to textual-inversion author rinongal for answering my questions)

```
The string has to be something which the model's tokenizer matches to a single token. I don't think this string will work. If you don't want to use things like "*" or "@" you can often use characters in other alphabets ("א", "ב") and some emojis.

The problem with using more than a single token is that it complicates the implementation significantly.
```

```
The embedding merging code actually lets you swap the placeholders in case there's a conflict, so if you're worried about that - there's no need to be. You can train everything with e.g. * and then just swap them out later when merging.
```

```
If you want to make it more user-friendly, you can just add some replacement code that changes the prompt before it is fed into the model. For example, you could add code which replaces the string "mypersonalword" with "*" and then continues as normal, in which case you'd be able to just use "mypersonalword" when prompting.
```

Merging implementation idea:
* with e.g. 10 embeddings assign each embedding a unique[1] single token placeholder i.e. "*", "@", "א", "ב" etc[2] [5]
* each embedding is also associated with the more user-friendly multi character string[3]
* existing placeholder token of embedding is replaced with the newly assigned placeholder token
* store the association between embedding (as in a name for it, the subject or w.e), assigned single character placeholder token, and user-friendly multi-character placeholder in some file[4]
* on GUI load merged embeddings file and file containing associations, some sort of list 

[1] find out how many unique characters are usable for upper limit on how many embeddings can be merged

[2] prefer more unique characters from other alphabets that are less likely to be included in the prompt by mistake

[3] this will need to be unique too, ideas for best format, consider that a list of placeholder words can be on the gui and probably clickable to insert into prompt box so it isn't required to be easy to type or very short, but still readable/recognisable 

[4] something like
```
[
  {
    'embedding_name': 'subjectA',
    'single_token': 'א',
    'friendly_placeholder': 'subjectA-CAFEBABE'
  },
  {
    'embedding_name': 'subjectB',
    'single_token': '@',
    'friendly_placeholder': 'subjectB-CAFED00D'
  },
  
  ...
  
]
```

[5] input to merge multiple embeddings could be similar

```
[
  {
    'embedding_name': 'subjectA',
    'embedding_filename': 'subjecta-embeddings.pt',
    'single_token': 'א',
    'friendly_placeholder': 'subjectA-CAFEBABE'
  },
  {
    'embedding_name': 'subjectB',
    'embedding_filename': 'subjectb-embeddings.pt',
    'single_token': '@',
    'friendly_placeholder': 'subjectB-CAFED00D'
  },
  
  ...
  
]
```
