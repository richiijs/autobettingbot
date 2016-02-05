# CSGODouble.com Auto betting bot
### Userscript that bets for you! Automates betting on [CSGODouble](http://www.csgodouble.com/) (works with [CSGOPolygon](http://csgopolygon.com/)).

Program interface

![Screenshot](http://imgur.com/YpfJ7CC.png)
![Screenshot](http://imgur.com/K5sPB4L.png)

Full site dark mode

![Screenshot](http://imgur.com/Mib1HtY.png)

## [![Install](https://i.imgur.com/hKHfyWz.png)](https://raw.githubusercontent.com/richiijs/csgodoublebot/master/csgodouble.com-automated.user.js)

## Requirements
Requires [Greasemonkey](http://www.greasespot.net/) on Firefox and [Tampermonkey](http://tampermonkey.net/) on Chrome.

## Is it safe?

Yes, you can use this script and don't worry about CSGODouble banning you - firstly, they don't really lose anything on you, secondary, checking for such activity is really troublesome.
Oh, you asked about '/send' command? Check the source - I tried to keep it clean and easy to understand.

## About

The script uses martingale to bet your coins, this means that with every lose it doubles bet value, changing it back to base after win. That, in theory, means you always win base value.

#### Why in theory?

In theory, because the situation takes player with infinite wealth (in our case - `coins`). With every lost bet the value is doubled - let's take `1` as base, here is possible scenario, 16 loses in a row:
```
 1 | Value:     1 | Total amount invested:      1 | Result: Lose
 2 | Value:     2 | Total amount invested:      3 | Result: Lose
 3 | Value:     4 | Total amount invested:      7 | Result: Lose
 4 | Value:     8 | Total amount invested:     15 | Result: Lose
...
12 | Value:  2048 | Total amount invested:   4095 | Result: Lose
...
16 | Value: 32768 | Total amount invested:  65535 | Result: Lose
17 | Value: 65536 | Total amount invested: 131071 | Result: Win
```
As you can see, it took `132071` coins just to get base `1` coin back.

#### Why don't you code 'train protection' (or any different form of protection) then?

If you ask such question, you don't even understand what is so-called `train/rainbow/anything protection` and why it is bullshit (in other words - doesn't work).

Let me show you another example, we are betting on `red`, here is current result history:
```
red, red, black, red, green, black, black, black, black, black
```
What should the script do in that situation - keep with red, switch to black?
The answer is - whatever it does the winning chance is exactly the same.
You may say something like - "Why?! You can see many streaks on the site, it is impossible!".
Well, it is possible, the site uses a system which chose `random` (at least very close to be random) number.

Let's simplify it to a coin toss (which is pretty similar) - we have 50% chance for heads, 50% chance for tails*.
We start tossing. Heads. What are the chances, after the first toss? 50/50! Tossing it again (and again...) won't change the chance a bit.

#### Does that mean I may lose everything in one unlucky streak?

Yes. That is what I'm trying to say. That also means you will eventually *lose everything* using this algorithm (algorithm, not the script itself, applies to any other script using this system).
To cheer you up a bit I can say that within gambling's very nature lays the possiblity of losing everything. All you can do is to play safe and use only funds you can afford to lose!

#### Why would I play the game if i can lose everything?

Because that doesn't mean you may not earn anything - calculate base value depending on your balance and risk you want to take.

**How to calculate**

``floor(balance / 2^(loses_in_a_row_you_want_to_cover + 1))``

Examples:
```
Balance: 10000
Safety: Medium, want to be safe even if the streak is 8 turns long
Base value: floor(10000/2^9) = 19
```
```
Balance: 250000
Safety: High-medium, want to be safe with 12 turns long streaks
Base value: floor(250000/2^13) = 30
```
```
Balance: 5000000
Safety: High, you are risking a lot of money, you want to be safe with 15 turns long streaks
Base value: floor(5000000/2^16) = 76
```

#### Okay, I think I understand... But why are streaks so common?

Remember, as the site uses computer randomness, it may feel strange, but that os how it works.
As a human you wouldn't except more than X `reds` in a row, because you are counting 10, 20, maybe 100 rolls and the outcome should give equal chances for both colors, but keep in mind that machine counts *thousands* more times rolls than you can - if in your calculations (100 rolls) `reds` may be chosen 2 times in a row, what is wrong with 20 `reds` streak in thousands of rolls?

In fact streaks like that are more natural for real randomness than you probably think, that is why people may call the site rigged, but it is not (I'm not defending the site, just the algorithm itself).


#### Why is 'Rainbow' set as the default option?

Because when you look on the history, you may think it's the safest option to go rainbow - the site don't pay me enough (in fact - they don't pay me at all :anguished:) to say it's true, but... Well, you may feel better!

*[They say it is not, but we don't care, right?](https://www.youtube.com/watch?v=AYnJv68T3MM)


## Changelog

1.40:

- Work with new sites.

1.31:

- Compatible with CSGOPolygon.com
- Two new modes - bet on random color, bet on last winning color

1.26:

- New bet system - `D'alembert`

1.25:

- New bet system - `Bet on green`, uses fibonacci sequence.

1.24:

- Great martingale

1.23:

- Base bet calculation based on given failsafe value


1.22:

- Auto reconnect (without reloading the page!);
- Switched to chat notifications - no longer need to open the console;
- Fixed an issue when the script bets twice right after starting;
- Fixed an issue when changing bet color;
- Fixed intervals not being cleared after stopping (backend change);
- Removed an option to bet negative values;


1.2:

- Dark theme


