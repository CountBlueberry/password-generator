# Password Generator

A small, single-file password generator that runs entirely in your browser. No install, no build step, no network requests.

## Usage

Open `index.html` in any modern browser (double-click it). That's it — it works offline.

- Adjust the options; a new password is generated on every change.
- Press **Enter** or **↻** to regenerate.
- **Copy** puts the password on your clipboard.
- Set **How many passwords** above 1 to get a batch, with per-item copy and **Copy all**.

## Options

| Option | Description |
|---|---|
| Length | 1–1024 characters (slider covers 4–128) |
| Lowercase / Uppercase / Digits | Standard character sets |
| Symbols | Toggle on/off; the symbol set itself is editable |
| Custom characters | Extra characters always added to the pool (e.g. `ąęł€`) |
| Exclude characters | Characters that will never appear |
| Exclude look-alikes | Removes `0 O o 1 l I \| ` ' "` |
| At least one from each selected set | Guarantees every enabled set is represented |
| No repeated characters | Each character used at most once (length ≤ pool size) |
| How many passwords | Generate up to 100 at once |

The strength meter shows estimated entropy: `length × log2(pool size)` bits.

| Entropy | Rating |
|---|---|
| < 40 bits | Weak |
| 40–59 bits | Fair |
| 60–89 bits | Good |
| ≥ 90 bits | Very strong |

## How the randomness works

- Uses `crypto.getRandomValues()` (Web Crypto API), which is backed by the operating system's cryptographically secure random number generator.
- Characters are chosen with **rejection sampling**, so there's no modulo bias: every character in the pool has exactly equal probability.
- When "at least one from each set" is on, one character is drawn from each set first, the rest from the full pool, then the result is shuffled with a Fisher–Yates shuffle using the same secure source.
- `Math.random()` is never used.

### Why no online random service?

Services like random.org produce good randomness, but a password fetched over the network is by definition known to the server (and potentially anything in between). Generating locally with the OS CSPRNG is both more private and cryptographically sufficient.

## Browser support

Any browser with the Web Crypto API — all current versions of Chrome, Edge, Firefox and Safari. If it's unavailable, the page shows an error instead of generating an insecure password.

## Project structure

```
PasswordGenerator.html   # the whole app: HTML, CSS and JS
README.md
```
