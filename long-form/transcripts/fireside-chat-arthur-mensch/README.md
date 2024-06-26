# Fireside Chat with Arthur Mensch
This is an interview between Elad Gil and Arthur Mensch, the CEO of Mistral AI.
## Audio
The audio is extracted from the following youtube video

https://www.youtube.com/watch?v=sQpeIuymJZ8

## Transcript
There are three versions of the reference transcript of this video:
1. `raw_fireside_chat_w_mistral_ceo_arthur_mensch.txt`<br>
This is the raw diarized transcript.
It looks like this:
<details>
  <summary>Click to show</summary>

    DYLAN FIELD

    Hi everybody. Welcome. Thank you so much for being here.
    I am so glad that we're able to host this at Figma. My name is Dylan Field. I'm the CEO ...

    ELAD GIL

    Oh, thanks. Thanks so much to Figma for hosting us, and thanks everybody for making it today. 

    ARTHUR MENSCH

    Thanks for having mere here ...
</details><br>

2. `nondiarized_fireside_chat_w_mistral_ceo_arthur_mensch.txt`<br>
This is the cleaned, nondiarized transcript. It's basically one big blob of text without newlines.This is the best option for benchmarking ASR & STT systems. It looks like this:
<details>
  <summary>Click to show</summary>
  
    Hi everybody. Welcome. Thank you so much for being here. I am so glad that we're able to host this at Figma. My name is Dylan Field. I'm the CEO and cofounder of ...
</details><br>

3. `diarized_fireside_chat_w_mistral_ceo_arthur_mensch.json`<br>
This is the diarized transcript. It's a list of dictionaries of the form:
<details>
  <summary>Click to show</summary>
  
    [
    {
        "speaker": "DYLAN FIELD",
        "text": "Hi everybody. Welcome. ..."
    },
    ...
    ]
</details><br>

## Methodology
The transcript was generate in the followng steps:

1. Copy the video's autogenerated transcript from Elad's [blog post](https://blog.eladgil.com/p/discussion-w-arthur-mensch-ceo-of)
2. While listening to the video, manually go over it and correct any mistakes
3. Use python to create the different forms
### Non diarized Transcript
<details>
  <summary>Code snipper</summary>
  

    import re
    nondiarized = text.replace("DYLAN FIELD", " ").replace("ELAD GIL", "" ).replace("ARTHUR MENSCH", "")
    nondiarized_no_newlines = re.sub("\n+", " ", nondiarized)
    nondiarized_no_newlines_no_extra_whitespace = re.sub(" +", " ", nondiarized_no_newlines).strip()

    with open("NonDiarized_Fireside_Chat_w_Mistral_CEO,_Arthur_Mensch.txt", "w") as f:
        f.write(nondiarized_no_newlines_no_extra_whitespace)
  
</details>


### Diarized Transcript
<details>
  <summary>Code snipper</summary>

    import re
    import json

    diarized_no_newlines = re.sub(r'\n', ' ', text)
    diarized_no_newlines_no_extra_whitespace = re.sub(" +", " ", diarized_no_newlines).strip()
    split_text = diarized_no_newlines_no_extra_whitespace.replace("DYLAN FIELD ", "<sep> DYLAN FIELD").replace("ELAD GIL ", "<sep>ELAD GIL" ).replace("ARTHUR MENSCH", "<sep>ARTHUR MENSCH").split("<sep>")
    turns = [segment for segment in split_text if segment]
    turns

    speaker_turns = []
    for turn in turns:
        if "DYLAN FIELD" in turn:
            speaker = "DYLAN FIELD"
        elif "ELAD GIL" in turn:
            speaker = "ELAD GIL"
        elif "ARTHUR MENSCH" in turn:
            speaker = "ARTHUR MENSCH"
        else:
            print("Error")

        speaker_turns.append({"speaker": speaker, "text": turn.replace(speaker,"").strip()})

    with open("Diarized_Fireside_Chat_w_Mistral_CEO,_Arthur_Mensch.json", "w") as f:
        json.dump(speaker_turns, f, indent=4)
</details>