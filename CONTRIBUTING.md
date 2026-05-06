# Contributing

Thanks for your interest in improving Awesome PII Redaction.

## Quality bar

This is a curated list, not an exhaustive index. Before opening a PR, ask:

1. **Would you actually use this in production, or recommend it to a colleague?** "Looked interesting on GitHub" is not enough.
2. **Is it specifically useful for PII detection, redaction, anonymization, pseudonymization, or closely adjacent privacy work?** Generic NER models, generic ML libraries, and unrelated security tools belong elsewhere.
3. **Is it actively maintained?** Last commit within the last ~18 months, or it's a stable enough reference that staleness doesn't matter (e.g. a foundational paper).
4. **Does it have meaningful adoption or a clear technical merit?** Stars are a weak signal, but combined with documentation, releases, and real users it tells a story. New work with a unique angle is welcome - please make the angle explicit in the PR description.

## What's typically declined

- Forks of existing entries that don't add a clearly different feature
- "Gist-grade" projects: a single notebook, no README, no releases
- Wrappers around an entry already on the list, with no new behavior
- Closed-source services that aren't widely benchmarked against
- Generic NER models with no PII focus and no clear PII-relevant performance numbers
- Blog posts behind paywalls or interstitials

## Format

Add entries to the matching section in alphabetical order, except where ordering by another criterion (e.g. language coverage, downloads) is already in effect for that section.

Use this format for a single-line entry:

```
- [Name](https://url) - One-sentence description, ending with a period.
```

If a project has both a repo and a notable secondary link (a paper, demo, docs site), inline the secondary link inside the description rather than creating a second bullet.

## PR checklist

- [ ] Entry added in the right section, in the right order
- [ ] Description is one sentence and ends with a period
- [ ] URL works
- [ ] License/language noted only when it's a meaningful differentiator (not boilerplate)
- [ ] No broken links or markdown errors (`npx awesome-lint` is a useful sanity check)
- [ ] If submitting a project you authored, disclose this in the PR description

## Code of conduct

Be kind. Disagreements about whether an entry belongs are normal - argue technical merits, not people.
