---
title: "Mathematics of Risk"
date: 2024-08-03
draft: false
summary: "The Black-Scholes-Merton equation is a model for pricing options. This equation revolutionized finance by providing a precise method for determining fair option prices, improving risk management and trading efficiency."
tags: ["HMM", "Black-Scholes", "Black-Scholes-Merton", "Risk", "Volatility", "Economics", "Economy"]
---

# The Evolution of Financial Modeling: From Bachelier to Modern Day

## Bachelier’s Model

### Early Beginnings

[Louis Bachelier](https://nl.wikipedia.org/wiki/Louis_Bachelier) (1870-1946), a pioneer in applying mathematics to financial markets, worked in the Paris stock market with a keen interest in options. Despite the long history of options, a **reliable pricing method** was missing, and traders typically bargained to set prices.

### The Mathematical Approach

Bachelier, already fascinated by probability, proposed a mathematical solution to this problem for his PhD thesis under [Henri Poincaré](https://en.wikipedia.org/wiki/Henri_Poincar%C3%A9). Surprisingly, Poincaré accepted this unconventional topic.

### The Discovery

Bachelier suggested that stock prices follow a normal distribution centered on the current price, spreading out over time. He realized this mirrored [Joseph Fourier](https://en.wikipedia.org/wiki/Joseph_Fourier)’s 1822 heat equation, dubbing it the "Radiation of Probabilities." By the end of his PhD, Bachelier had developed a method to price options, predating Einstein’s random walk concept. However, his work went largely unnoticed due to the lack of immediate financial application.

## Ed Thorpe

### From Blackjack to Wall Street

In the 1950s, [Ed Thorpe](https://en.wikipedia.org/wiki/Edward_O._Thorp), a physics PhD student, identified a money-making opportunity in Las Vegas by inventing Card Counting for blackjack. This strategy, based on tracking cards, initially earned him significant profits until casinos countered by using multiple decks.

### Application to Stock Market

Thorpe then applied his strategy to the stock market, founding a hedge fund that achieved a 20% annual return for 20 years. He introduced [dynamic hedging](http://docs.finance.free.fr/Options/Dynamic_Hedging-Taleb.pdf), a method to protect against losses through balanced transactions.

I've written a very simple explanation of of Dynamic Hedging at the end of the article, hope you find it helpful.

However, Thorpe found Bachelier’s model insufficient, noting stock prices are influenced by business performance. In 1967, he developed a more accurate option pricing model incorporating this drift, refining Bachelier’s work until 1973.

## Black-Scholes & Merton Equation

### Revolutionizing Finance

In 1973, [Fischer Black](https://en.wikipedia.org/wiki/Fischer_Black) and [Myron Scholes](https://en.wikipedia.org/wiki/Myron_Scholes) introduced an equation for option pricing, with Robert Merton independently contributing.

They constructed a risk-free portfolio of options and stocks, akin to Thorpe’s delta hedging, proposing that in an efficient market, such a portfolio should yield the risk-free rate.

### The Improved Model

They built on Bachelier’s model by including both random price movements and a general trend (drift), creating a widely recognized equation in finance. This provided a clear formula for pricing options based on various parameters, revolutionizing trading practices.

### Recognition

In 1977, [Merton](https://en.wikipedia.org/wiki/Robert_C._Merton) and Scholes received the Nobel Prize in Economics for their contributions, with Black acknowledged posthumously.

## Jim Simons - Medallion Fund

### From Mathematics to Markets

With the Black-Scholes formula public, [Jim Simons](https://en.wikipedia.org/wiki/Jim_Simons), a mathematician, sought new ways to identify market inefficiencies. He founded Renaissance Technologies in 1978, leveraging machine learning to find stock market patterns.

### The Medallion Fund

Simons hired top scientists, including [Leonard Baum](https://en.wikipedia.org/wiki/Leonard_E._Baum), to utilize Hidden Markov Models and other data-driven strategies. The Medallion Fund became the highest-returning investment fund ever, challenging the efficient market hypothesis.

## History of Options and Options Trading

### Early Examples

Options likely originated to manage risk. The earliest known contract dates to 600 BC with Greek philosopher [Thales of Miletus](https://en.wikipedia.org/wiki/Thales_of_Miletus), who secured the right to rent olive presses at a fixed price, profiting from a predicted bumper crop.

### Types of Options

A **call option** grants the right but not the obligation to buy an asset at a set price, useful when expecting price increases.

Conversely, a **put option** provides the right but not the obligation to sell an asset at a set price, ideal for anticipated price declines.

Options offer benefits such as limiting downside risk, providing leverage, and serving as a hedge.

## Financial Theories and Practices

### Efficient Market Hypothesis

[The Efficient Market Hypothesis](https://en.wikipedia.org/wiki/Efficient-market_hypothesis) (EMH) asserts that stock prices reflect all available information, making it impossible to consistently outperform the market. According to EMH, stocks are always fairly valued, so superior returns require taking on higher risks.

### Delta (Dynamic) Hedging

Delta hedging aims to minimize directional risk from price changes in the underlying asset. The goal is to achieve a delta-neutral position, avoiding directional bias. The formula for a hedged portfolio is `π = V - ∆S`, allowing the creation of synthetic options through dynamic trading based on changes in option and stock prices.

{{<github repo="aestheticvoyager/black-scholes-merton" >}}
