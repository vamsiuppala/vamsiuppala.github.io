# Speaker Recognition and Translation

I was reading Vicki Boykis' blog post - [Both Pyramids Are White](https://vickiboykis.com/2024/03/13/both-pyramids-are-white/) on how groups collectively think. The entire post revolves around a [1971 Russian Video](https://www.youtube.com/watch?v=_LYe58b-3HM) that is difficult for any non Russian speakers to understand. 

In a Vicki fashion, she encouraged folks to transcribe and translate this video into Russian. 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">^ BTW if anyone wants to uSe ArTifIciAl inTellIgEnCE, transcribing this video in Russian and translating into English would be a fantastic and practical use-case!</p>&mdash; vicki (@vboykis) <a href="https://twitter.com/vboykis/status/1767918512981364986?ref_src=twsrc%5Etfw">March 13, 2024</a></blockquote> 
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


And so I did.

OpenAI's [Whisper](https://platform.openai.com/docs/guides/speech-to-text) is generally able to both transcribe and translate audio files directly using a easy to use Audio API. But, since Whisper model doesn't do speaker diarization (distinguish between multiple speakers). Since this video had multiple speakers, I had to break the process into multiple steps:

1. Download the YouTube video as a .wav file using pytube
2. Diarize and transcribe the video using assemblyai
3. Colloquialize the old Russian-to-English translation into modern speak using OpenAI's chat API

### Code to get you there 
[GitHub Repo Link](https://github.com/vamsiuppala/translate-video.git) with code, transcribed and translated files.

**First, import required packages**

```python
import pytube as pt
import os
from deep_translator import GoogleTranslator
import assemblyai as aai
import config as cfg
from openai import OpenAI
```

**define location to store files**
```python
directory = 'files/'
os.makedirs(directory, exist_ok=True)
```

**download the video**
```python
video_name = 'me_and_others'
video_url = "https://www.youtube.com/watch?v=_LYe58b-3HM"
  
yt = pt.YouTube(video_url)
stream = yt.streams.filter(only_audio=True)[0]
stream.download(filename = directory + '/' + video_name + '.wav')
```

**transcribe it**
```python
aai.settings.api_key = cfg.keys['aai_api_key']
transcriber = aai.Transcriber()

audio_url = (
"files/me_and_others.wav"
)

config = aai.TranscriptionConfig(speaker_labels=True, language_code='ru')
transcript = transcriber.transcribe(
	audio_url,
	config
)

# output translation
with open(directory + '/' + 'me_and_others_transcribed' + '.txt', 'w') as file:

	for utterance in transcript.utterances:
		file.write(f"Speaker {utterance.speaker}: {utterance.text} \n")
	file.close()
```

**peep into transcription**
```txt
Speaker A: Я знаю, как на мёд садятся мухи. Я знаю смерть, что рыщет всё губя. Я знаю книги истин и слухи. Я знаю всё. Но только не себя. Жан Славиен. Этот фильм приглашает вас взглянуть на себя со стороны. И тогда, увидев себя в других, вы, быть может, задумаетесь. Что знаем мы о себе? О своей психологии? Насколько мы самостоятельны в своих суждениях? и как влияют на нас другие люди. 
Speaker B: ПОЗИТИВНАЯ МУЗЫКА. 
Speaker C: Очень часто, когда начинается судебная, прокурорская или адвокатская деятельность молодого юриста, происходит очень много серьёзных ошибок, иногда и казусов, связанных с тем, что свидетельским показанием передаётся слишком большое и, даже я скажу иначе, некритическое отношение к ним проявляется. А свидетельские показания — это показания живых людей, показания, которые даются Хотя и в равных условиях, как это иногда бывает, когда люди видят одно и то же, однако, давая показания, дают совершенно разное объяснение происходящему и совершенно по-разному толкуют виденные ими факты. Происходит это потому, что, в общем-то, складывается так, что людям вообще, скажем, в утешении этому обстоятельству, людям вообще свойственно ошибаться. Я думаю, что вот теперь и самое время проверить, Насколько сильна человеческая память? И каковы свойства этой памяти? И в какой мере память каждого из вас склонна к отклонениям? Итак, количество нападавших. 
```

**translate it**
```python
with open(directory + '/' + 'me_and_others_transcribed' + '.txt', 'r') as file:
	text = file.read()
	file.close()

# Chunk the text into sentences split by '\n' - to minimize token length of each translation
chunks = text.split('\n')

# translate each chunk
translated_chunks = [GoogleTranslator(source='auto',
									  target='en').translate(chunk) 
									  for chunk in chunks
									  ]

# join the chunks back
translated_transcript = '\n'.join(translated_chunks)

# output translation
with open(directory + '/' + 'me_and_others_transcribed_and_translated' + '.txt',
		  'w') as file:
	file.write(translated_transcript)
	file.close()
```

**translated transcript**
```text
Speaker A: I know how flies land on honey. I know death that prowls around, destroying everything. I know the books of truths and rumors. I know everything. But not yourself. Jean Slavien. This film invites you to look at yourself from the outside. And then, seeing yourself in others, you may think about it. What do we know about ourselves? About your psychology? How independent are we in our judgments? and how other people influence us.
Speaker B: POSITIVE MUSIC.
Speaker C: Very often, when a young lawyer begins the judicial, prosecutorial or advocacy work, a lot of serious mistakes occur, sometimes incidents related to the fact that too much testimony is conveyed and, even I will say differently, an uncritical attitude towards them is manifested. And testimony is the testimony of living people, testimony that is given, although in equal conditions, as sometimes happens when people see the same thing, however, when giving testimony, they give completely different explanations of what is happening and completely different interpretations of what they saw data. This happens because, in general, it turns out that people in general, let’s say, in consolation of this circumstance, people generally tend to make mistakes. I think that now is the time to check, How strong is human memory? And what are the properties of this memory? And to what extent is the memory of each of you prone to deviations? So, the number of attackers.
```

**make it colloquial**
```python
client = OpenAI(
	api_key = cfg.keys['openai_token']
	)

# define a colloquialization function that calls OpenAI with a system context

def colloquialize(sys_def, model, content):
	c = client.chat.completions.create(
		model=model,
		messages=[
			{"role": "system", "content": sys_def},
			{"role": "user", "content": content}
		]
	)
	return (c.choices[0].message.content)

# define system role
translator_sys = "You are a professional English language expert who can convert old formal English writeups into colloquial semi formal language. You maintain a semi professional tone and retain as much context as possible."

# Open transcript as text file

with open(directory + '/' + 'me_and_others_transcribed_and_translated' + '.txt', 'r') as file:
	text = file.read()
	file.close()

# Chunk the text into sentences split by '\n'
chunks = text.split('\n')

# translate each chunk
colloquialized_chunks = [colloquialize(translator_sys, 
									   "gpt-3.5-turbo", 
									   chunk) for chunk in chunks]

# join the chunks back
colloquialized_transcript = '\n'.join(colloquialized_chunks)

# wite it into a text file
with open(directory + '/' + 'me_and_others_transcribed_translated_colloquialzied' + '.txt',
		  'w') as file:

	file.write(colloquialized_transcript)
	file.close()
```

**colloquialized transcription**
```
Speaker A: I understand how flies get attracted to honey. I'm familiar with the concept of death lurking around and causing havoc. I'm well-versed in both facts and gossip. I have a lot of knowledge, but not about you, Jean Slavien. This movie encourages you to take a look at yourself from a different perspective. By recognizing yourself in others, you might start reflecting on your own identity. What do we truly understand about ourselves? How does our psychology work? To what extent are we able to form our own opinions independently, and how much are we influenced by others?
Speaker B: Upbeat tunes.
Speaker C: Often, when a new lawyer starts in court or as a prosecutor, they can make some pretty big mistakes. Sometimes, they might rely too much on witness testimonies without questioning them enough. Witnesses are real people who give their accounts, but even in similar situations, they can have vastly different perspectives on what happened. Why? Well, people are prone to making mistakes. It's worth considering how reliable our memories really are and how easily they can be influenced. It's important to question the accuracy of these memories, especially in legal settings.
```


### How can we make it better?
As you can tell, neither the translation nor its colloquial version are perfect. I believe this could be significantly improved with a better frame system prompt and using advanced versions of the GPT models, or the competitors from Mistral, Meta, Anthropic et al. 

Let me know if you have any ideas on how to improve either speaker diarization or the translation.


We could also turn the video into a readable blog post, with screenshots and sectional speaker summarization. It probably involves a ton of prompt engineering and manual edits, but could save hours of manual transcription. There's one great recent example:

### Turn a technical video into a writeup
[Emmanuel](https://twitter.com/mlpowered) and [Erik](https://twitter.com/ErikSchluntz) do a great job transcribing and using prompt engineering to create a blog post out of a two hour video tutorial on Tokenization by [Karpathy](https://twitter.com/karpathy). 

**Video:**
<iframe width="560" height="315" src="https://www.youtube.com/embed/zduSFxRajkE?si=TG8FcnQmK4CSYwWa" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Claude 3 Opus is great at following multiple complex instructions. <br><br>To test it, <a href="https://twitter.com/ErikSchluntz?ref_src=twsrc%5Etfw">@ErikSchluntz</a> and I had it take on <a href="https://twitter.com/karpathy?ref_src=twsrc%5Etfw">@karpathy</a>&#39;s challenge to transform his 2h13m tokenizer video into a blog post, in ONE prompt, and it just... did it<br><br>Here are some details: <a href="https://t.co/ABmMvIkoQ0">pic.twitter.com/ABmMvIkoQ0</a></p>&mdash; Emmanuel Ameisen (@mlpowered) <a href="https://twitter.com/mlpowered/status/1764718705991442622?ref_src=twsrc%5Etfw">March 4, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

In their words,
>
To produce it, we used a transcript of the video along with screencaps taken every few seconds. We then chunked it into N parts, and passed each part through the prompt below. No further editing was done.

The resulting model-generated course - [LLM Tokenization](https://hundredblocks.github.io/transcription_demo/)

**Prompt passed on to Claude 3 Opus:**
```

Human: Here is a transcript of a video, including screenshots at different timestamps.
The transcript was generated by an AI speech recognition tool and may contain some errors/infelicities.
Your task is to transform the transcript into an html blog.
The writing style of the blog, including desired balance between text and code is illustrated in a screenshot in <desired_writing_style>.
The visual style of the blog, including page layout, font, headings and image styles are described in <desired_visual_style>.

{transcript_with_name}
<desired_writing_style>
{desired_writing_style}
</desired_writing_style>
<desired_visual_style>
{desired_visual_style}
</desired_visual_style>

This transcript is noisy. Please rewrite it into an html format for an blog using the following guidelines:
- output valid html
- insert section headings and other formatting where appropriate
- use styling to make images, text, code, callouts and the page layout and margins look like the example in <desired_visual_style>
- remove any verbal tics
- if there are redundant pieces of information, only present it once
- rewrite conversational content in the style shown in <desired_writing_style>, including headings to make the narrative structure easier to follow along
- each transcript includes too many images, so you should only include the most important 1-2 images in your output
- choose images that provide illustrations that are relevant to the transcript
- prefer to include images which display complete code, rather than in progress
- when relevant transcribe important pieces of code and other valuable text
- if an image would help illustrate a part of a transcript, include it
- to include an image, insert a tag  with src="frames/hh_mm_ss.jpg"  (ie "frames/00_12_35.jpg") copying the exact image timestamp inserted above the image in the transcript
- add captions to images
- do not add any extraneous information: only include what is either mentioned in the transcript or the images

Your final output should be suitable for inclusion in a textbook.

Assistant: <!DOCTYPE html>

```

They've added more details to a [GitHub repo](https://github.com/hundredblocks/transcription_demo.git), but there aren't many details on the steps to chunk the video into parts and the level of human effort that went into everything.

