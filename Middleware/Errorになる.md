# Errorになるやり方

## OK

```
export const config = {};

export const config = {
matcher: [],
};

export const config = {
matcher: ["/"],
};

```

## NO Good

```
export const config = {
matcher: [""],
};
```
