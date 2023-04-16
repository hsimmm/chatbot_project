# chatbot_project
kommunicate.io 영화 완성
-텔레그램에 구현

아나콘다
pandas
sentence-transformers
sklearn   |      scikit-learn


1: conda --v
2: conda create -n chatbot python=3.8
3: conda env list
4: conda activate chatbot
6: pip install pandas
8: pip install sentence_transformers
9: pip install sklearn
10: pip install scikit-learn














4/16
심리상담 프로그램을 만들어서 구현(유사도)

새로운 챗 만듦
import streamlit as st
from streamlit_chat import message
import pandas as pd
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity
import json

@st.cache(allow_output_mutation=True)
def cached_model():
    model = SentenceTransformer('jhgan/ko-sroberta-multitask')
    return model

@st.cache(allow_output_mutation=True)
def get_dataset():
    df = pd.read_csv('wellness dataset1.csv')
    df['embedding'] = df['embedding'].apply(json.loads)
    return df

model = cached_model()
df = get_dataset

st.header('심리상담 챗봇')

if 'generated' not in st.session_state:
    st.session_state['generated'] = []

if 'past' not in st.session_state:
    st.session_state['past'] = []

with st.form('form', clear_on_submit=True):
    user_input = st.text_input('당신: ', '')
    submitted = st.form_submit_button('전송')
    
if submitted and user_input:
    embedding = model.encode(user_input)
    
    df['distance'] = df['embedding'].map(lambda x: cosine_similarity([embedding], [x]).squeeze())
    answer = df.loc[df['distance'].idxmax()]
    
    st.session_state.past.append(user_input)
    st.session_state.generated.append(answer['챗봇'])
    
for i in range(len(st.session_state['past'])):
    message(st.session_state['past'][i], is_user=True, key=str(i) + '_user')
    if len(st,session_state['generated']) > i:
        message(st.session_state['generated'][i], key=str(i) = '_bot')
        
  (주피터)
