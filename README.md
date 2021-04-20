# spoken_wikipedia_dutch

To download the dataset, run the following bash commands

`
!(if [ -d "20000_mijlen/*" ]; then \
     echo "Data exists in directory '20000_mijlen'. If the directory is empty or corrupted, remove it and re-run this cell." ;\
 else \
     rm -f audio_1.zip* audio_2.zip* transcript.txt* ;\
     wget -q --show-progres https://github.com/JeannieDaniel/spoken_wikipedia_dutch/releases/download/v1.0/audio_1.zip ;\
     wget -q --show-progres https://github.com/JeannieDaniel/spoken_wikipedia_dutch/releases/download/v1.0/audio_2.zip ;\
     wget -q --show-progres https://github.com/JeannieDaniel/spoken_wikipedia_dutch/releases/download/v1.0/transcript.txt ;\
     unzip -o audio_1.zip ;\
     unzip -o audio_2.zip ;\
     mkdir -p 20000_mijlen ;\
     mv audio_1/* 20000_mijlen/ ;\
     mv audio_2/* 20000_mijlen/ ;\
     rm -rf audio_1 audio_2 audio_1.zip audio_2.zip ;\
 fi)
`

To map each .wav and its accompanying text in the dataset, run the following:

`
def get_data(metadata = "transcript.txt", maxlen=50):
  """ returns mapping of audio paths and transcription texts """
  data = []
  with open(metadata, encoding="utf-8") as f:
      for line in f:
          id = line.split('|')[0]
          text = line.split('|')[1]
          length = float(line.split('|')[-1].split('\n')[0])
          data.append({"audio": id, "text": text, 'audio_length': length})
  return data
  data = get_data('transcript.txt', 200)
  `
  
  The .wav files are currently 32-bit. To convert them to 16-bit, run the following:
  
 `
 import soundfile
 import os
 def convert_wav_files_to_16_bit(directory):
    for file in os.listdir(directory):
        if(file.endswith('.wav')):
            name = file.rsplit('.', 1)[0]
            data, samplerate = soundfile.read(directory + file)                
            soundfile.write(directory + name + '.wav', data, samplerate, subtype='PCM_16')
  
  convert_wav_files_to_16_bit('20000_mijlen/')
  `
  
  
