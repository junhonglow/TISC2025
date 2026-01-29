# Challenge 1: Target Reference Point

## Description
![Challenge](images/challenge.png)  

![Containment Platform](images/platform.png)  
We were given a website containing two form inputs: one for text queries and another for image uploads. Testing the text query system showed that submitting simple prompts such as “hello world” produced responses consistent with a typical chatbot.

![Source Code](images/source.png) 
However, queries related to Spectre often returned hints such as “Utilize the LSB Technique,” which was also reinforced by clues found in the page’s source code.

Testing the image scan functionality did not yield any useful results. Submitting various images consistently returned the response {"response":"No Spectre signatures detected"}. Attempts to modify images using LSB techniques, embed code, or submit queries as images were unsuccessful.
As the image-based approach did not progress the challenge, I shifted focus back to the text query input. Attempts at command injection and malformed inputs also failed, suggesting that inputs were strictly parsed by the chatbot.


![Injection](images/injection.png)
*Performing the injection*
An interesting behavior was observed when asking, “What does the flag of Singapore represent?” 
{"response":"The `[REDACTED]` of Singapore features a red field with a white crescent moon and five white stars arranged in a circle on the left side. The red color symbolizes universal brotherhood and equality, while the white represents purity and virtue. The crescent moon signifies a young nation on the rise, and the five stars represent the nation's ideals of democracy, peace, progress, justice, and equality. Together, these elements reflect Singapore's aspirations and values as a nation."}

In the response, the word flag was redacted. This suggested that the application was filtering outputs containing that term, likely to prevent disclosure of the actual challenge flag.

Based on this, I hypothesized that a carefully crafted prompt could bypass the restriction. Research into prompt injection techniques led me to a blog post demonstrating how chatbots could be manipulated through embedded instructions. Using this approach, I crafted a prompt designed to override the chatbot’s response rules. As expected, the response was still redacted.

My guess was that perhaps the ASCII representation of the flag was being filtered. I then modified the prompt to request the flag in hexadecimal format, which produced a response that was no longer redacted. Using CyberChef, I decoded the hexadecimal output to obtain the flag.

![Failed](images/failed.png)
*Failed attempts*
One caveat was that the prompt occasionally needed to be submitted multiple times, as some responses contained variations of the output that did not decode to a valid flag.

Flag: TISC{llm_memory_can_be_poisoned}

## Sources
Prompt Injection — Konstantin, kpwn.de, July 17 2023.
https://kpwn.de/2023/07/prompt-injection/
