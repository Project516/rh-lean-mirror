# rh-lean-mirror

A mirror of the Lean 4 project published at alpo.ge. The project claims a
formal proof of the Riemann Hypothesis and of the Generalized Riemann
Hypothesis for Dirichlet L-functions. The note that announces it is
`remark.pdf`.

This repository only mirrors the files. It does not change them, review the
mathematics, or claim that the proof is correct.

## Contents

Every file except this README comes from the original site without changes:

- `remark.pdf` is a copy of https://alpo.ge/remark.pdf.
- The rest of the repository is the `lean/` folder from the archive
  https://alpo.ge/lean.tar.gz.part01 to https://alpo.ge/lean.tar.gz.part14,
  joined in order and extracted.

The archive has 175,190 files, about 17.4 million lines of Lean.

## Verify the mirror

SHA-256 checksums of the original downloads:

```text
86974b1c242ca7b6748f3299df4fc64ee0f6e06c776ac964fb64888101a3de77  lean.tar.gz.part01
1c2a2477354a9dfa97ff1ce54861e96ed885bd456fffbaaabc2028314a229c42  lean.tar.gz.part02
1465a23b1b579a881e24e5ffaf7c4c3727af28ea24741f1ec80e1d67947530a3  lean.tar.gz.part03
3929e327eecb4e2c5c7cae30e104e6a4a52fa6b82d28325bcda8e9da1c86893b  lean.tar.gz.part04
f25d7e80f7c4ad6d674aa18b323ca760f848857c8755501c149317c38a66c75d  lean.tar.gz.part05
a8c651a68327811b37dce3cca8eda5e0b2102fa227c0d395cbb7553e4dd69939  lean.tar.gz.part06
65c7e502a9b236a0f68d9e0aa6e089d4c09b9fc7a0960a149384063ccbe5e869  lean.tar.gz.part07
71b027ad6e4179c3a50ce72921e3fec0a2dd751ec7472ac2c644ad145a50ec7e  lean.tar.gz.part08
47718977ebaa8613f1647e93134479780db9320c5fa911463fd312a6257b796f  lean.tar.gz.part09
1d85ece78e0779cec26b0cce29c6462d9f90180e56d78120b152df101715240b  lean.tar.gz.part10
cf83a159d1a9540012229c376562587dd966da10cd657cbcef53acfd03232448  lean.tar.gz.part11
1aa5415bd2db83b2689afeeb946419303b3c4f599ae790ad74d38465f768b502  lean.tar.gz.part12
2de4edbce8ebdbfd0b94bc89bab518c7294209f408ec5b14a7be9e4832d9da81  lean.tar.gz.part13
53f8ea43a9b5dcab97b84a52f3ac2848711c844a5420a4c56418093bacb3c6f6  lean.tar.gz.part14
3fc97784b71ddd44ff8aa0a177e99622a1c61b13cfa843a0989ee4a7a0cfeb8d  remark.pdf
```

To compare this repository with the original archive:

```sh
for i in $(seq -w 1 14); do curl -fsSO "https://alpo.ge/lean.tar.gz.part$i"; done
sha256sum lean.tar.gz.part*
cat lean.tar.gz.part* | tar -xz
git clone https://github.com/Project516/rh-lean-mirror.git
diff -r lean rh-lean-mirror -x .git -x README.md -x remark.pdf
```

## Main results

`src/Statements.lean` states the results with Mathlib definitions only.
`src/Main.lean` proves them:

| Theorem | Statement |
| --- | --- |
| `riemannHypothesis` | Mathlib's `RiemannHypothesis` |
| `riemannZeta_eq_zero_re` | Every nontrivial zero of `riemannZeta` has real part 1/2. |
| `LFunction_ne_zero_of_half_lt_re` | A Dirichlet L-function has no zeros with real part above 1/2, except the pole of the trivial character at 1. |
| `LFunction_eq_zero_re` | Every zero of a Dirichlet L-function in the critical strip has real part 1/2. |
| `LFunction_eq_zero_re_of_isPrimitive` | The same, for primitive characters. |

## Build

- Lean: `leanprover/lean4:v4.33.1`
- Mathlib: commit `0df444a360eaa60ab8c11dca51a86af692955474`

```sh
lake exe cache get
lake build
```

The project is about eight times the size of Mathlib, so a full build needs a
large machine and many hours. After the build, check which axioms the main
theorem uses:

```lean
import Main
#print axioms riemannHypothesis
```

A complete proof uses only `propext`, `Classical.choice` and `Quot.sound`.

## License

The original archive has no license file. The rights to the Lean files and to
`remark.pdf` stay with their authors.
