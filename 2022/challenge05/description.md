# Description 

To not tire the reindeer, Papa Noel wants to leave the maximum number of gifts by making the least number of trips possible.

He has an array of cities where each element is the _number of gifts he can leave there_. For example, [12, 3, 11, 5, 7]. He also has a _limit on the number of gifts_ that can fit in his bag, and finally, the _maximum number of cities_ that his reindeer can visit.

As he doesn't want to leave a city half-way, _if he can't leave all the gifts that are from that city, he doesn't leave any there_.

Create a program that tells him the _highest sum of gifts that he could distribute_, taking into account the maximum number of gifts and the maximum number of cities he can visit. For example:

```js
const giftsCities = [12, 3, 11, 5, 7];
const maxGifts = 20;
const maxCities = 3;

// the highest sum of gifts to distribute
// visiting a maximum of 3 cities
// is 20: [12, 3, 5]

// the highest sum would be [12, 7, 11]
// but it exceeds the limit of 20 gifts and he
// would have to leave a city half-way.

getMaxGifts(giftsCities, maxGifts, maxCities); // 20 (12 + 3 + 5)
```

If it is not possible to make any trips that satisfies everything, the result should be 0. More examples:

```js
getMaxGifts([12, 3, 11, 5, 7], 20, 3); // 20
getMaxGifts([50], 15, 1); // 0
getMaxGifts([50], 100, 1); // 50
getMaxGifts([50, 70], 100, 1); // 70
getMaxGifts([50, 70, 30], 100, 2); // 100
getMaxGifts([50, 70, 30], 100, 3); // 100
getMaxGifts([50, 70, 30], 100, 4); // 100
```

To consider:

maxGifts >= 1
giftsCities.length >= 1
maxCities >= 1
The number of maxCities can be greater than giftsCities.length

# Solutions

## Alternative I

```js
function getMaxGifts(giftsCities, maxGifts, maxCities) {
  const calculate = (maxCities, giftsCities, maxGifts, total = 0) => {
    if (maxCities === 0 || giftsCities.length === 0) return total;

    const totslDesdeInicio = calculate(
      maxCities - 1,
      giftsCities.slice(1),
      maxGifts,
      total + giftsCities[0]
    );

    const totalDesdeSegundo = calculate(
      maxCities,
      giftsCities.slice(1),
      maxGifts,
      total
    );

    return totslDesdeInicio > totalDesdeSegundo && totslDesdeInicio <= maxGifts
      ? totslDesdeInicio
      : totalDesdeSegundo;
  };

  return calculate(maxCities, giftsCities, maxGifts);
}
```

[Download](https://github.com/jpaddeo/tdd-adventjs/2022/challenge05/solution1.js)

## Alternative II

```js
function getMaxGifts(giftsCities, maxGifts, maxCities) {
  giftsCities = giftsCities
    .sort((x, y) => y - x)
    .reduce((result, _, idx) => {
      if (idx) giftsCities.unshift(giftsCities.pop());

      idx = giftsCities
        .slice(0, maxCities)
        .reduce((acc, curr) => (acc += curr), 0);
      idx <= maxGifts && result.push(idx);

      idx - giftsCities[maxCities - 1] <= maxGifts &&
        result.push(idx - giftsCities[maxCities - 1]);
      return result;
    }, []);
  return Math.max(...(giftsCities.length ? giftsCities : [0]));
}
```

[Download](https://github.com/jpaddeo/tdd-adventjs/2022/challenge05/solution2.js)
