Technical task for the AI intern position at Fanvue.

# Instructions

Please clone this repo, upload it to your own Github account, and complete the tasks below. Once you are done, please send us the link to your public repo.

You may complete the tasks in whatever manner you see fit, as long as it is in python or javascript/typescript - including jupyter notebooks, or javascript using a nodejs server. As such, you **may not** use OpenAI's playground in your final submission. (You can still use it for testing, of course.)

Time to complete: 2 hours.

# Part I

One of Fanvue's core features is messaging between creators and fans.

You are tasked with creating a proof-of-concept (PoC) for a bot that takes in a fan's message and produces a list of the fan's personal details (like city they live in, how old they are, etc.), as well as a fan's personal preferences that are mentioned in that message.

For example, if the fan's message contains the sentence "I like strawberries", the bot will produce the output "fan likes strawberries."

The email we sent should contain a OpenAI API key. This will allow you to use the [OpenAI's API](https://platform.openai.com/docs/api-reference/chat/create?lang=python) to build the bot. We recommend you use [the OpenAI Node.js or Python packages](https://platform.openai.com/docs/api-reference/introduction) for the PoC.

## First task:

Create a PoC for a bot that takes in any message, and does the above. Each user preference should be in a new line, with a star emoji ⭐ at the beginning.
For example, if a fan's message is _"My name is Fred, and I enjoy long walks on the beach."_, the final output should be:

- ⭐ Fan's name is Fred.
- ⭐ Enjoys long walks on the beach.

Note: the words don't have to be exactly the same as in the example - what's important is that the main preferences are outputed.

The messages written by the fan can also contain information that is not related to the fan's preferences or personal details - in which case the bot should output a consistent response. This consistent response can be anything you prefer - it can be the words "no new details.", or a particular character - what's important is that this output is consistent across tests.

For example, if the fan writes _"Hello, how are you?"_, the bot should reply with the consistent response, because the fan did not provide any new details about themselves in that message.

## Second task:

Extend the PoC's functionality such that the fan details and preferences outputted by the bot are stored, and the bot does not output the same preferences again, even if the fan mentions them again.

For example:

- The fan writes _"I like long walks on the beach"_ in one message.
- The bot outputs the fan preference as per the first task.
- Then the fan writes _"I like long walks on the beach"_ again.
- The bot does not output this preference again, because it is aware of the fact that the fan mentioned this previously. Instead, it outputs the consistent response.

# Part II

Oftentimes you will have to manipulate data to prepare it for fine-tuning, or to extract insights.
In this repo you will find a csv file, _fan_creator_chat.csv_, with messages between a fan and a creator. The task is to export a jsonl file with the example format below (taken from the [openAI fine-tuning walkthrough](https://platform.openai.com/docs/guides/fine-tuning/preparing-your-dataset)).

The assistant role should be taken by the creator, and the user role should be taken by the fan.

The system message for this task is: "Jada is a creator on Fanvue, chatting with one of her fans."

```json
{"messages": [{"role": "system", "content": "Jada is a creator on Fanvue, chatting with one of her fans."}, {"role": "user", "content": "What's the capital of France?"}, {"role": "assistant", "content": "Paris, as if everyone doesn't know that already."}]}
{"messages": [{"role": "system", "content": "Jada is a creator on Fanvue, chatting with one of her fans."}, {"role": "user", "content": "Who wrote 'Romeo and Juliet'?"}, {"role": "assistant", "content": "Oh, just some guy named William Shakespeare. Ever heard of him?"}]}
{"messages": [{"role": "system", "content": "Jada is a creator on Fanvue, chatting with one of her fans."}, {"role": "user", "content": "How far is the Moon from Earth?"}, {"role": "assistant", "content": "Around 384,400 kilometers. Give or take a few, like that really matters."}]}
```

In the attached csv, you will notice that the fan and/or the user can send multiple messages that are tied to one another. For example, the creator wrote:

- Message 1: _I've loved drawing since I was a kid. It was always my way of expressing myself._ 🥰
- Message 2: _Over time, I just kept at it and started experimenting with different mediums._

Then the fan replies with: _That's really inspiring. I've always struggled to stick with one hobby._

In these cases, you need to concatenate message 1 and message 2 before you pass them in the format above to the jsonl file.
