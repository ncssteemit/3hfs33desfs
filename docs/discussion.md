# Discussion

This document assumes a minimum knowledge of Steem and Steemit. If you don't know what Steem is, please check out the [Steem White Paper](https://steem.io/SteemWhitePaper.pdf) and the [Steemit.com](https://steemit.com) website.

## Bots on Steemit

Bots, while controversial, have become a fact of Steemit usage. They maximise the rewards of users by voting strategically. They do game the system, but since the system is a game (and one of the co-creators is extremely ambivalent on the subject) it is tolerated.

However it's a concern that good bots are currently only the hands of a few users. While this is their purview, it's ours to use whatever we can within the system to compete with them. They do not really want much high quality bot competition, as it lowers their potential take, but if we don't level the playing field they will dominate the platform too much.

Where the general tools exist to add automatic curation, they come at a cost, which can be a mandatory vote for the services' posts, direct payment, or even by relinquishing data control by tracking. To use these services you must also hand over your private posting keys, which is comes with its own risks.

### So what is this?

The goal of this project is to bring a high quality and fully customisable bot to anyone who is willing to put in the time to set up a simple server and write their own customisations. I'd like to see bot programming guilds of people helping each other.

We recognise that there is a investment of time and patience required to set this up and ever effort has been made to make this as easy as possible. In the future it would be ideal to have a super simple, consumer grade interface but at present there is some overhead.

Be encouraged though that it is certainly possible to set up without any existing technical knowledge, just a user's knowledge of Steem.

## How it works

_Note, none of this is implemented yet, it's still in the ideation stage_

See my [original Steemit post here](https://steemit.com/curation/@personz/5qnnnx-free-open-source-steemit-bot-proposal-and-question) where this idea was proposed.

### Definition of problem

To state the problem directly; **to automatically vote for posts that I would post for anyway while also maximising my curation rewards.**

To break this down, we can separate two distinct curation goals which are in conflict with each other: cultural curation and strategic curation.

#### Cultural curation

Cultural curation votes for posts based on the content of the post and other related cultural, and thus social aspects. When you curate culturally, you vote for a post because you liked the content, or even if you didn't like it, you thought it was well writen. In other words, it contributes to the culture of Steem in a positive way, however you wish to define it.

There are a lot of other peripheral influences for a cultural vote. Perhaps you didn't read a post, or didn't much like it, but want to support the author for a different reason, such as you know and like them, or they are new, or any number of possibilities. A broad definition of cultural curation would include them all.

#### Strategic curation

Strategic curation votes for posts that will yeild the best curration reward, and over time votes in a pattern which maximises reward in the long term. Any user of Steem will know that the "best" posts do not always get the highest rewards, and sometimes "undeserving" posts get very high rewards. The existence of betting, games, competitions, and the presence of whales, etc. complicates matter significantly.

#### In conflict but also collaboration

If we do not want to disregard cultural curation completely, we must have some element of strategy. And it makes sense to maximise the Steem Power of your account, not just for personal gain, but also so that your cultural curation have more weight, thus making your votes stronger and ultimately promoting the kind of content you wish to see, or in other words, contributing towards creating a culture on Steem which you are part of.

A wholistic solution will include both aspects, cultural and strategy. They interact with each other. Cultural aspects effect payout indirectly, by attracting voters based on content, so they will always be worth considering even if one is concerned purely with curation rewards.

### Solution to the problem

#### Reducing complexity

_Note: voting is assumed to be at full 100% power here_

When we visit at Steemit.com, we see new posts. If we visit periodically we will have a collection of new posts which we could potentially vote for. We'll call this set ```N```.

We must reduce the complex question of whether or not to vote on a post to a simple yes or no for each post. To reduce it a little less, say we calculate a numeric score for each post, and choose the post to vote on based on some criteria. This scoring process will be multi-dimensional but we can make each dimension simple enough to calculate with a simple test, and then combine each individual score.

These individual scores will be based on **metrics**, such as word count, number of votes already cast, whether the author is followed, etc. Metrics are combined into a single score using an algorithm of values, weights and conditions, which we'll just call an algorithm. The metrics are facts of the Steem posts and environment, whereas the algorithm is a customizable process which each user can tweak to make their own bot curate as they would like it to curation. See the [algorithm doc page](/docs/algorithm.md) for a detailed, technical look at this.

In the simpliest case, we would choose to vote on any post which had a score greater than a certain value. This value is called a threshold. But it is a bit more complex than that, as we'll see.

#### Constrained by vote frequency

Let's say we have scored all the posts in ```N```. Since voting is **rate limited**, each time a vote is cast the next vote's power is reduced, and it is restored slowly in periods of voting inactivity.

From the [Steem White Paper](https://steem.io/SteemWhitePaper.pdf), pg 18:

> Through rate limiting, stakeholders who vote more frequently have each vote count for less than stakeholders who vote less frequently. [...] a user’s voting power decreases every time they vote and then regenerates as time passes without voting. [...] Note that voting power rapidly drops off during periods of continuous voting, and then slowly recovers.

From a strategy point of view, it makes sense to restrict the number of votes cast. Too few and you cannot have an cultural impact nor get enough curation rewards; too many and you dilute your impact. This is clearly an optimisation problem (potentially solved by AI). But Steem posters have variously reported annecdotally that the sweet-spot is somehwere between 25 and 50 per day. So we can make sure to try to vote at least 25 times and no more than 50 per day.

Another strategy factor is that the earlier you vote for a post which _will be_ successful, the larger your curation reward. So clearly it is prudent to check for new posts often.

Though on average you should vote every 30 to 60 minutes, the best posts will most likely not be made so regularly. A potential solution to this dilemma is to use a **time windowed score average as a threshold** to determine if a post has a high enough score to vote on, and to sample N (i.e. see if there are new posts) very often, say every 10 minutes. The window length will be an important factor which will require manual tweaking or optimisation, but we could use a window period of 12 hours as a starting point.

If there are a few good posts in one hour they will all get voted on and will raise the windowed average so that within that window the posts need to be better to be voted on. Similarly, if posts score very low for several hours in a row, the next post which is a bit better will get voted on. This solves the problem if finding an fixed, absolute threshold.

It also implies that the scoring system is relative and that, for example, a score of 40 does not mean anything except in relation to another score, say 20. 40 is twice as "suitable" as 20, but beyond that we don't need to know anything, nor do we need to.

Finally, since the window size is hard to find, we can allow it to grow or shrink depending on how many suitable posts were found. If too few posts were found, e.g. less than 25, then we neeed to decrease the window size to allow it to respond faster to changes. If too many posts were found, e.g. more than 50, we should increase the window size to stop the threshold from sinking or rising too quickly.