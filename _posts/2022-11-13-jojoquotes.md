---
layout: blog_post
title: "Making a Jojo Quote Generator with GPT2"
tags:
- Python
- PyTorch
show: true
---

How to Make a Jojo Quote Generator with GPT2

#### *やれやれだぜ!  (good grief!)*

If you clicked on this article, you are probably already familiar with the Japanese animated show “JoJo's Bizarre Adventure”, which as the title suggests, features a bizarre plot and is well known for its characteristic over-the-top and bigger-than-life voice lines, which are often quoted among its fans. 

Recently I became interested in replicating the style of these lines by making use of GPT2. This is a bit outside of my area of expertise, so I thought a short DIY side project would help me learn more about generative models by getting hands-on experience. My friends and I found the results quite entertaining so I decided to share them with you, and also how I got there.

Here are examples of the types of sentences the model generates (The starting seed is in square brackets):

```
[This] is my last chance!
[Jojo,] that's why I have to break out!
[Wonder] why you're taking so long to make the coffee.
```

To make these sentences, we need training data, and this is where I quickly ran into trouble. I could not find any transcripts for the show from my initial searches, so I tried to find a way to directly get the subtitles from Netflix. Luckily for me, using the [Language Reactor](https://www.languagereactor.com/) extension it is possible to export the subtitles into excel tables.

After downloading the subtitles for each episode, I placed them into a data folder and got to training. For this step, I closely followed [this](https://medium.com/analytics-vidhya/fine-tune-gpt-2-225c09400cb6) article to fine-tune a pre-trained GPT2.

I played around with the hyper parameters, and I found that training over the data for two epochs, and using the following parameters for generation produces the most entertaining results.

```python
model.generate(
	model_input, 
	do_sample=True,   
	top_k=25, 
	max_length = 32,
	top_p=0.90, 
	num_return_sequences=2
	)
```

Since Jojo’s Bizarre Adventure is a Japanese show, I also tried training a Japanese model by switching the language to Japanese on Netflix and using that as the training data. I also used the same parameters, with similar levels of success, after looking over the results with a native Japanese speaker.

You can find the Jupyter notebooks used for training [here](https://github.com/VSteinborn/jojo-gpt2-generator) on Github.

And that’s it! You can also have the model running as a bot on a Twitch Channel, by using [Twitch IO](https://twitchio.dev/en/latest/) or in a Discord channel using [python-discord](https://github.com/weibeu/python-discord). If you found this article useful, please consider starring the Github repository with the Jupyter notebooks.

Until next time!




