# My thoughts on LLM usage

_Published ..._

_Heads up this one is an opinion piece._

_This is my thoughts and feelings at the date of publishing. These thoughts and feelings might change and they don't have to be yours._

Lately my boss has been drinking the LLM koolaid, touting 5x, 10x or even 20x
efficiency gains through using LLMs to write code. The conversations which this
mentality has created has forced me to really reflect on what AI means to me.

I use LLMs almost daily myself in my home assistant setup. I have a small model
running locally on a cheap, lower power GPU to do basic low consequence
reasoning and summarising for automations. For an example, I usually do the
washing in the morning and hang it outside on the clothesline. Then I have the
issue that I then go about my day and forget about the clothes until I remember
them last minute at night and hurry out to bring them in. This results in
clothes being outside in the sun for 8+ hours.

So I have an LLM which digests the current weather forecast then attempt to
predict how many hours it will take for the washing to dry. Then home assistant
sets a timer and sends me a notification when my clothes are ready to be
brought in. The following is the output of it's reasoning ([Qwen3.5 4B distilled
through Claude Opus
4.6](https://huggingface.co/Jackrong/Qwen3.5-4B-Claude-4.6-Opus-Reasoning-Distilled)),
as an aside it's funny how it converts the kph wind estimates to mph.

```markdown
To determine the minimum hours needed to dry clothes before sunset, I need
to analyze the weather forecast and current time.

## Time Analysis:

- Current time: 2026-03-31 11:18:18 UTC+11:00
- Sunset time: 2026-03-31T08:16:04+00:00 = 2026-03-31T19:16:04+11:00
- Remaining time before sunset: 19:16 - 11:18 = **7 hours 58 minutes**

## Weather Conditions (today):

- All conditions are sunny throughout the morning
- Temperature is rising from 22°C to 28°C
- Wind speeds range from 17-22 mph with gusts up to 31 mph
- UV index ranges from 4-6
- No precipitation expected

## Minimum Drying Hours Calculation:

For the minimum drying time, I'm considering optimal conditions:

- **1 hour**: May be too aggressive for most fabric types
- **1.5 hours**: Borderline for many garments
- **2 hours**: Safe for quick-dry conditions with strong wind and sunshine

However, given that you have 8 hours before sunset and the optimal weather
conditions, I recommend **6 hours** as the minimum practical time.

This recommendation accounts for:

- Conservative margin of safety
- Variation in fabric drying rates
- Potential wind variability
- Ensures complete drying to prevent mildew
- Well within the 8-hour window available

If your clothes are lightweight or you're in an urgent situation, you
could potentially do it in 4-5 hours, but 6 hours provides a reliable
```

Now I could very well create an algorithm that's able to deterministically
calculate the number of hours required to dry my washing for any given weather
forecast, however getting a fuzzy result from an LLM is usually going to be
good enough and is quicker to implement.

To me LLMs are simply a tool, a 'normal' tool as [Cory Doctorow might put
it](https://pluralistic.net/2026/03/12/normal-technology/) and yes this tool
has downsides like any other

- Hallucinations 
- High computing cost for training and running
- Dubious interpretations of copyright law when collecting training data
- Questionable copyright status of generated content
- And more

Though with time and I do believe that there are going to be at least partial
solutions to most of these problems, such as freely licensed training data,
distilling to make small models that can run efficiently on consumer hardware
with acceptable results, etc. Therefore there might be niche use cases where
LLMs might make sense.

But there is an issue which I don't believe is being talked about enough and
that is the masking of individual expression. 

## AI Homogenization

The thing that scares me the most about the widespread adoption of LLMs to
summarise, edit and create text is the way in which it likes to convert
everything into a standardized phrasing and formatting. This paves over nuance
and careful consideration until everything sounds like it's been written by an
overenthusiastic middle level manager. Documentation, stories, opinions, code,
as soon as an LLM is introduced into the mix, even if it's just as an external
aid, it influences us and everything starts to slowly homogenize towards the
LLM's style. 

Unfortunately this affects people who don't even use LLMs, as long as they are
reading content which has been influenced by LLMs. The way in which people
express themselves is influenced by the environment in which they interact with
and so if one interacts with LLM content then there's the potential for them to
subconsciously gravitate towards the LLMs style.

Thus none of my content on this blog, mastodon or code on Codeberg/GitHub ever
has been or is ever going to be created or edited with an LLM. With the
exception of directly quoting an LLM such above.

The best defence we have right now against AI homogenization is disclosure.
If you use LLMs in any capacity to create a work, please disclose it. That way
at least your audience is somewhat aware that this is the voice and reasoning
of an LLM and not of a human.

