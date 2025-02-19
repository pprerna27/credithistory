# credithistory
Predict the customer who likely to default.
import pandas as pd

# Sample DataFrame
data = {
    'tag': ['h1', 'h2', 'h3', 'h4', 'h1', 'h2', 'h4', 'h4', 'h3', 'h4', 'h5', 'h6', 'h7', 'h8'],
    'text': [
        'Chapter 1', 'Subtitle 1.1', 'Subsubtitle 1.1.1', 'Content 1.1.1.1',
        'Chapter 2', 'Subtitle 2.1', 'Content 2.1.1.1', 'Content 2.1.1.2', 'Subsubtitle 2.1.1', 'Content 2.1.1.3',
        'Subsection 2.1.1.1', 'Subsubsection 2.1.1.1.1', 'Subsubsubsection 2.1.1.1.1.1', 'Subsubsubsubsection 2.1.1.1.1.1.1'
    ]
}

df = pd.DataFrame(data)

# Function to create structured DataFrame
def create_structured_df(df):
    structured_data = []
    cat1, cat2, cat3, cat4, cat5, cat6, cat7, content = None, None, None, None, None, None, None, []

    for index, row in df.iterrows():
        tag = row['tag']
        text = row['text']

        if tag == 'h1':
            if cat1 is not None:
                structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])
            cat1, cat2, cat3, cat4, cat5, cat6, cat7, content = text, None, None, None, None, None, None, []
        elif tag == 'h2':
            if cat2 is not None:
                structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])
            cat2, cat3, cat4, cat5, cat6, cat7, content = text, None, None, None, None, None, []
        elif tag == 'h3':
            if cat3 is not None:
                structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])
            cat3, cat4, cat5, cat6, cat7, content = text, None, None, None, None, []
        elif tag == 'h4':
            if cat4 is not None:
                structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])
            cat4, cat5, cat6, cat7, content = text, None, None, None, []
        elif tag == 'h5':
            if cat5 is not None:
                structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])
            cat5, cat6, cat7, content = text, None, None, []
        elif tag == 'h6':
            if cat6 is not None:
                structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])
            cat6, cat7, content = text, None, []
        elif tag == 'h7':
            if cat7 is not None:
                structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])
            cat7, content = text, []
        elif tag == 'h8':
            content.append(text)

    if cat1 is not None:
        structured_data.append([cat1, cat2, cat3, cat4, cat5, cat6, cat7, ' '.join(content)])

    structured_df = pd.DataFrame(structured_data, columns=['cat1', 'cat2', 'cat3', 'cat4', 'cat5', 'cat6', 'cat7', 'content'])
    return structured_df

# Create the structured DataFrame
structured_df = create_structured_df(df)
print(structured_df)