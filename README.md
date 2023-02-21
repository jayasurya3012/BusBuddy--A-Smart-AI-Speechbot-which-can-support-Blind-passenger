# BusBuddy--A-Smart-AI-Speechbot-which-can-support-Blind-passenger
The aim of this project is to support blind people to know when their next train/flight will be, and their arrival and destination

## Idea and Descripytion

The purpose of this initiative is to aid passengers who have special needs, such as the visual impairments or the elderly.
Specifically, a question-and-answer model is given data that specifies when buses and flights leave and arrive at their respective locations. 
This is a fine-tuned Distilbert model that has already been trained to answer questions. The more information there is in the corpus, the better 
the model will perform.


## Installation

Use the package [pip](https://pypi.org/project/SpeechRecognition/) to install SpeechRecognition.

```bash
pip install SpeechRecognition
```
Use the package [pip](https://pypi.org/project/gTTS/) to install gTTS.

```bash
pip install gTTS
```
Use the package [pip](https://pypi.org/project/transformers/2.1.0/) to install transformers(2.1.0).

```bash
pip install transformers==2.1.0
```
## Usage
### Import packages

```python
import speech_recognition as sr
from gtts import gTTS
import transformers
from transformers import pipeline
import time
import os
import datetime
import numpy as np
import pandas as pd
```

### Loading the data
```python
df=pd.read_csv("Flight_Timings.csv") #mention the path which contains the downloaded CSV file

```
### Pre-processing of the data
We transform various data values, such as converting location names to their abbreviations, to improve the model's capacity to recognize and the user's ability to pronounce these tokens. Following preprocessing, we turn these data into sentences and append them to a paragraph to construct the corpus.
```python
record1=""
for i in range (0,len(df)):
    df['From'][i]=df['From'][i].replace('United Kingdom','UK')  
    df['From'][i]=df['From'][i].replace('United Arab Emirates','UAE')
    df['To'][i]=df['To'][i].replace('United Kingdom','UK')
    df['To'][i]=df['To'][i].replace('United Arab Emirates','UAE')
    
    # Here in record1, we create the Corpus required to know the arrival time, departure time of bus/plane from onne place to another
    record1+="Bus from "+df['From'][i]+" to "+df['To'][i]+" arrives at "+df['Arrival Time'][i]+" and departs at "+df['Departure Time'][i]+". "

```
### Constructing new corpora from current data in order to add extra characteristics
We use the same data, but employ an variety of methods to collect fresh data and produce new corpora. This improves the model's intelligence, efficiency, and usability.
```python
record2=""
record3=""
from_d={}
to_d={}

for i in range(0,len(df)):
    if(df['From'][i] not in from_d.keys()):
        from_d[df['From'][i]] = list([df['To'][i]])
    else:
        from_d[df['From'][i]].append(df['To'][i])

for i in from_d:
    txt = ""
    for j in from_d[i]:
        txt+= j+", "
    # In record2, we create a Corpus which contains no. of bus/plane from a place(destination)    
    record2+="There are "+str(len(from_d[i]))+" buses from " + i +". "
    # In record3, we create a Corpus which contains all the destinations of each bus/train
    record3+= "bus from "+i +" goes to places "+txt+". "
```
### BusBuddy Code
The driver code for this project is available in the [repository](https://github.com/jayasurya3012/BusBuddy--A-Smart-AI-Speechbot-which-can-support-Blind-passenger/blob/main/BusBuddy.ipynb).
