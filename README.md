# eval-medcalc-bench

**MedCalc-Bench**

> ⚠️ **Third-party eval.** This is a `register/` pointer in inspect_evals — the task code lives in an external repository of unaudited provenance and will execute on OpenEvalz infrastructure. Onboarding it is a security review, not a packaging task.

**Paper:** https://arxiv.org/abs/2406.12036

Evaluates whether language models can perform clinical calculations from
free-text patient notes. Each sample provides a patient note and asks the model
to compute a specific clinical value across 55 calculators (lab tests, physical
quantities, risk/severity scores, dosages, and dates). Rule-based calculators
are scored by exact match; equation-based calculators are scored within the
authors' tolerance band. Faithful Inspect AI port of the NeurIPS 2024 benchmark.

## At a glance

| | |
|---|---|
| Upstream | [`register/medcalc-bench`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/register/medcalc-bench) |
| Group | — |
| Total samples | 0 |
| Execution class | `plain` |
| Cost class | `low` |
| Flags | no sandbox, no network |
| Tags | medical, reasoning, Knowledge |

### Tasks

| Task | Samples |
|---|---|
| `medcalc_bench` | 0 |

### External assets

_None declared upstream._

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/medcalc_bench \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
