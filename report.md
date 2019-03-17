# Predicting Single-Day Ski Resort Lift Ticket Prices

## 1. Introduction

### 1.1. Background

Downhill skiing and snowboarding are fun recreational activities that have been associated with health benefits
[[1]]. Recently, the ski lift ticket prices have been steadily rising [[2]], while the industry might have been
facing difficult times dealing with its own challenges [[3]].

As a window price for a single-day ski lift ticket for one person can exceed $200 at the top resorts,
it is often the most important factor to reckon with when a skier or snowboarder wants to budget a one day trip to
a mountain. Even when it comes to a multi-day vacation to a remote resort, the lift ticket expenses can be
still easily comparable to the total cost of lodging and transportation cost of getting to and from the resort.
Given the fact that for the most people such prices could not be viewed as nuance expenses,
it is naturally to expect that the ski lift price must hold to the value and quality of the experience
that the resort promise to deliver.

A one-day, adult ski lift ticket is not the only option that a resort provides these days.
There are half-day tickets, tickets for kids, beginner packages, lift and rentals packages, etc. Most importantly,
there is typically an option to buy a season pass. The pass options also vary significantly in their prices
and terms. Indeed, there seem to be some evidence that resorts are pushing for their season pass sales while
making the single-day lift tickets prohibitively expensive [[2]]. Even if this is the case, and the single-day lift
tickets are priced above their market values, it is still reasonable to assume that the single-day ticket price reflects
the quality of the resort. This could be, for example, because a holder of ski pass is expected to spend several days
at the company resorts and, thus, spend more on other snow activities (cross-country skiing, snow-tubing, snowshoeing,
sled riding, etc.), as well as on dinning, nightlife, and lodging. If so, the resorts that have better off-mountain
infrastructure can provide more appealing season pass options, and this, in its turn, can justify the further increase
of a single-day ticket price. Whatever is the marketing strategy of a particular resort, a single-day lift ticket
price is an important metric to compare the resorts, and it is the figure that many customers pay attention to.

When it comes to the ticket price, itt is natural to assume that the resort mountain infrastructure,
such as the number of ski lifts, and the quality of moutain terrain,
such as the vertical drop and the number of trails, should be priced-in into the lift tickets. However, it is less obvious
whether the off-mountain infrastructure, such a number of restaurants, bars, and hotels, affect the ticket prices.

### 1.2. Business Problem

This project aims to predict a ski resort single-day ski ticket price based on the available
information about the resort. The main question that will be addressed here is whether the resort
off-mountain infrastructure, i.e. the number of restaurants, bars, and hotels, could improve the prediction
outcomes and could be, therefore, considered to be an important factor of the perceived ski resort value.

### 1.3. Interest

Ski resorts might be interested in knowing the expected market value for the lift ticket
at their resorts, to achieve any competitive advantage. This is especially relevant as the ski prices
changes at least on an annual basis. The ability to relailbly evaluate market prices is important for dynamic pricing.
If any influence of the off-mountain infrastructure on the lift ticket prices is found, the resorts may consider
investing heavily into new venue development to improve the perceived resort value
and justify further increase of the ticket prices.

Skiers and snowboarders might be interested in knowing
how justified are the ticket prices at a particular resort in order, for example, to choose a resort
that have great moutain slopes terrain but does not necessary provide a lot of dinning options.

## 2. Data

The following datasets will be used in building and evaluating the prediction models.

* [www.skiresort.info][4] contains an extensive list of the U.S. ski resorts with the basic information
for each resort such as the vertical drop,
the base and peak elevations above the sea level,
the total length of trail runs and their relative difficulty levels,
number of ski lifts,
as well as the lift ticket price. This list can be web-scraped and used as a starting dataset.

* [www.onthesnow.com][5] contains another table on most current prices at North America ski resorts
and can be web-scraped to improve the price relevance.

* [Foursquare Places API][6] can be used to retrieve
the number of nearby restaurants, bars, and hotels within a given radius from a given resort location.
The number of nearby venues will be used as a measure of the resort off-mountain infrastructure attractivness.

## References
1. [Why Skiing Is a Ridiculously Good Workout][1]
2. [How Did Lift Tickets Become So Expensive?][2]
3. [Is the ski industry on a slippery slope?][3]
4. [Ski resorts in the United States of America][4]
5. [North America Resort Lift Tickets - Most Current Prices][5]
6. [Foursquare Places API][6]

[1]: http://time.com/5118770/is-skiing-a-good-workout/
[2]: https://snowboarding.transworld.net/photos/when-did-ski-resort-lift-tickets-become-so-expensive/
[3]: https://www.bbc.com/news/business-42110566
[4]: https://www.skiresort.info/ski-resorts/usa
[5]: https://www.onthesnow.com/north-america/lift-tickets.html
[6]: https://developer.foursquare.com/
