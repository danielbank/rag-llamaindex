# Retrieval-Augmented Generation with LlamaIndex

zA RAG-powered Web App demo using [Next.js](https://nextjs.org/) and [LlamaIndex](https://www.llamaindex.ai/).

## Slide Deck

This project was created as the demo for an accompanying talk: **Retrieval-Augmented Generation with LlamaIndex** that was presented at the [Phoenix AI Devs](https://www.meetup.com/phx-ai-devs/) meetup. The [slide deck](slides/rag-llamaindex.pdf) is included in this repo in the `/slides` folder.

## Bootstrapping a Next.js LlamaIndex Project

[Create LlamaIndex App](https://www.npmjs.com/package/create-llama) is a CLI tool that helps us to quickly bootstrap a RAG-powered web app. It can be run with the following command:

```sh
npx create-llama@latest
```

The following options produce a minimal Next.js app that uses the local file system (`./data`) for its knowledge base and also stores the document embeddings in the local file system. This is suitable for demo purposes but is not performant because we are not leveraging a vector database.

```
✔ What is your project named? … my-app
✔ Which template would you like to use? › Chat
✔ Which framework would you like to use? › NextJS
✔ Would you like to set up observability? › No
✔ Please provide your OpenAI API key (leave blank to skip): … <openai_key>
✔ Which data source would you like to use? › Use local files (.pdf, .doc, .docx, .xls, .xlsx, .csv)
(navigate to material-ui-issues.csv)
✔ Would you like to add another data source? › No
✔ Please provide your LlamaIndex Cloud API key (leave blank to skip): … <llamaindex_key>
✔ Would you like to use a vector database? › No, just store the data in the file system
✔ Would you like to build an agent using tools? If so, select the tools here, otherwise just press enter ›
✔ How would you like to proceed? › Start in VSCode (~1 sec)
```

## Example RAG Use Case: Land of Sies

The knowledge base in the `./data` folder contains information about Land of Sies characters in Markdown (`.md`) and PDF (`.pdf`) formats. Land of Sies was a story I wrote in high school which is unknown outside of a small group of friends and people who visit my old blog (~0 people). As such, [ChatGPT](https://chat.openai.com/) has no information about this content in its training data.

Asking [ChatGPT](https://chat.openai.com/), "Who is Mooflor?" produces an uncertain response:

```
The term "mooflor" does not seem to be a
widely recognized or defined term in
conventional dictionaries or general internet
sources. It's possible that "mooflor" could be
a username or handle on social media
platforms, but specific details about an
individual or entity using this handle were
not found in the search results.

If "mooflor" is related to a specific context,
individual, or entity you are thinking of,
providing additional details might help narrow
down the search. It could be a unique
username on platforms like TikTok, Instagram,
or another social media site, where niche or
lesser-known influencers and creators often
use distinctive handles.

To find out more about "mooflor," you might
consider checking major social media
platforms directly or providing more context
regarding where you encountered this term.
```

By utilizing RAG with the knowledge base, we can prime the agent with information about Land of Sies, and it can answer more definitively:

```
Mooflor is a mischievous goblin-like
creature in the world of MOOFLOR. He enjoys
scaring people by hiding behind rocks and
jumping out unexpectedly. Mooflor dislikes
the Happies and their joyful activities, often
causing chaos and despair with his tricks and
plots. One of his notable schemes was
stealing Erty's happy wand. Mooflor sometimes
collaborates with Ukhed, a grumpy giant. His
emoticon is >:€.
```

We can ask specific questions about the content, such as "Who is the fastest character in Land of Sies?":

```
The fastest character in the Land of Sies
is Flambytoes. Flambytoes is known for his
swift speed and unique ability to fly. As a
messenger of the Happies, he is often seen
zipping around the realm, delivering
messages and news. Flambytoes is also very
loyal to Sies, the spirit of Happy, and is
always ready to assist in spreading happiness.
His emoticon is ^_^.
```

We can also ask specific questions about the content that bleed into other domains, and ChatGPT can still provide an authoritative answer, such as "Is Flambytoes faster than a camel?"

```
Yes, Flambytoes is faster than a camel.
Flambytoes is described as the fastest
character in the Land of Sies, with the
ability to fly using his rocket feet.
Camels are known for their endurance
rather than speed, so Flambytoes would
definitely outpace a camel in a race.
```

If the question is more indeterminant, we might get both answers with a degree of uncertainty expressed in the answer, "Is Flambytoes faster than an F16?":

- An F16 is faster than Flambytoes:

```
In the context of the Land of Sies and the
character of Flambytoes, Flambytoes is
depicted as the fastest character with rocket
feet that allow him to fly. While Flambytoes
is a fantastical character with extraordinary
speed within his realm, it is important to
note that an F16 fighter jet is a real-world
military aircraft designed for high-speed
combat missions. In a real-world scenario,
an F16 would be significantly faster than
Flambytoes.
```

- Flambytoes is faster is faster than an F16:

```
In the context of the Land of Sies and the
character of Flambytoes, who has rocket feet
and is known for being the fastest character
in the realm, it is likely that Flambytoes
would be faster than an F16 fighter jet.
Flambytoes' ability to fly with rocket feet
and his reputation for speed suggest that he
could outpace even a high-speed aircraft
like an F16 in the fantastical world of Sies.
```
