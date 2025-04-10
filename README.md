# seo_generator.py

from youtubesearchpython import VideosSearch
import random

def clean_word(word):
    return ''.join(char for char in word if char.isalnum())

def generate_seo(topic):
    search = VideosSearch(topic, limit=10)
    results = search.result()['result']

    titles = [video['title'] for video in results]
    keywords = set()

    for title in titles:
        for word in title.lower().split():
            word = clean_word(word)
            if word.isalpha() and len(word) > 3:
                keywords.add(word)

    selected_keywords = random.sample(list(keywords), min(10, len(keywords)))
    hashtags = ['#' + kw for kw in selected_keywords]

    title = f"{topic} - {random.choice(['असली कहानी', 'छुपी हुई सच्चाई', 'जो आपने नहीं सुनी होगी'])}"
    tags = ', '.join(selected_keywords)
    description = f"{topic} पर एक खास वीडियो। जानिए इस जनजाति की अनकही बातें।\n\nKeywords: {tags}\n\nHashtags: {' '.join(hashtags)}"

    return {
        "title": title,
        "tags": tags,
        "description": description,
        "hashtags": hashtags
    }

# Testing
if __name__ == "__main__":
    topic = input("Enter your video topic: ")
    seo = generate_seo(topic)
    print("\nSuggested Title:", seo["title"])
    print("Tags:", seo["tags"])
    print("Description:\n", seo["description"])