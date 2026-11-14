# clone-lin-maker-daijoin

Experimental **LIN** clone of MakerDAO / Sky `DaiJoin.join` / `DaiJoin.exit` — the
ERC20 DAI adapter (`rad = mul(ONE, wad)`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/join.sol` sha256 `96fceff4511aa976db5c6b144d83f4dd1bde832dd2f49fd6e54e69343b9583f9` blob `42b01c91c8f2a6b776440e16dea2c94497d966b0`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_daijoin.lin`](src/lin_maker_daijoin.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_daijoin_c11.c`](test/oracles/maker_daijoin_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_daijoin_kernel.c`](test/oracles/maker_daijoin_kernel.c)
- **Proofs (lin-open only):** `examples/maker_daijoin/` and `make maker-daijoin-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256 rad/wad/ray. Not a production DaiJoin replacement.

Named goldens (`RAY'=1e9`):

| id | call | value |
|---|---|---|
| JOIN rad | wad=1000 adapter=2e12 usr=5 token=1000 | **1000000000000** |
| JOIN from' | adapter after | **1000000000000** |
| JOIN to' | usr after | **1000000000005** |
| JOIN token' | ERC20 after burn | **0** |
| EXIT token' | live=1 wad=1000 token=7 | **1007** |
| CAGE join | live=0 | **ok=1** |
| CAGE exit | live=0 | **ok=0** |

`live` / `from_dai` / `to_dai` / `token` / `wad` are arguments (Solidity uses
storage `live`, `vat.move`, `dai.burn` / `dai.mint`). `1e10 * 1e9` fail-closes
while naive i64 wraps. Join after cage is accepted (Maker semantic).

## Reproduce proofs (in lin-open)

```
make maker-daijoin-proof
```

## Not claimed

Uint256 rad/wad/ray / mainnet DaiJoin / ERC20 mint-burn host / `vat.move` host parity.
LIN replacing Maker, Sky, DaiJoin, or the EVM. TVL or dollar savings. zk soundness.
`GemJoin` ilk slip. Hidden adapter storage.
