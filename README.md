# Hubble Exchange
Perpetual futures exchange on Avalanche.

### One-Time vyper setup
Vyper compilation with hardhat takes a ton of time and is performed on every run (no caching). Therefore, we place .vy files outside the contracts directory and manually compile and dump the abi and bytecode in files that are then picked up in the tests.

```
python3 -m venv venv
source venv/bin/activate
pip install vyper==0.2.12
```

### Compile
```
npm run vyper-compile && npm run compile
```

### Documentation
```
npx hardhat docgen
```
Open `./docgen/index.html` in a browser.
