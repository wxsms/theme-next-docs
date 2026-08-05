---
title: NexT 8.29.0 Released
date: 2026-08-05 07:18:03
---

<!-- Release notes generated using configuration in .github/release.yml at v8.29.0 -->

## What's Changed
### 💥 Breaking Changes
* Remove animejs by @stevenjoezhang in https://github.com/next-theme/hexo-theme-next/pull/982
### ⭐ Features
* Configure MathJax overflow handling by @stevenjoezhang in https://github.com/next-theme/hexo-theme-next/pull/986
```diff
math:
  mathjax:
    enable: false
    # Available values: none | ams | all
    tags: none
+    # Available values: overflow | scroll | scale | truncate | linebreak
+    display_overflow: scroll
```
* refactor: use Firestore REST API (4352696)
```diff
firestore:
  enable: false
  collection: articles # Required, a string collection name to access firestore database
-  apiKey: # Required
  projectId: # Required
```
* Deprecate body_scrollbar.stable option (940b226)
```diff
body_scrollbar:
  # Place the scrollbar over the content.
  overlay: false
-  # Reserving space for the scrollbar gutter even if the content is not overflowing.
-  stable: false
```
* Deprecate lazyload option (d54ab89)
```diff
-# Vanilla JavaScript plugin for lazyloading images.
-# For more information: https://apoorv.pro/lozad.js/demo/
- lazyload: false
```
* Replace Anime.js scrolling with native APIs by @stevenjoezhang in https://github.com/next-theme/hexo-theme-next/pull/981
* Support MathJax 4 by @stevenjoezhang in https://github.com/next-theme/hexo-theme-next/pull/985
* Support optional async and defer attributes in script helpers by @wherewhere in https://github.com/next-theme/hexo-theme-next/pull/952
* Improve Algolia search concurrency (5c7574f)
### 🐞 Bug Fixes
* Disable Fancybox hash handling with PJAX by @stevenjoezhang in https://github.com/next-theme/hexo-theme-next/pull/983
* Fix TOC height calculation for wrapped headings by @stevenjoezhang in https://github.com/next-theme/hexo-theme-next/pull/984
### 🌀 External Changes
* Update dependency eslint to v10.8.0 by @renovate[bot] in https://github.com/next-theme/hexo-theme-next/pull/972
* Update actions/setup-node action to v7 by @renovate[bot] in https://github.com/next-theme/hexo-theme-next/pull/974
* Update dependency c8 to v12 by @renovate[bot] in https://github.com/next-theme/hexo-theme-next/pull/975
* Update actions/setup-python action to v7 by @renovate[bot] in https://github.com/next-theme/hexo-theme-next/pull/976
* Update actions/labeler action to v7 by @renovate[bot] in https://github.com/next-theme/hexo-theme-next/pull/977
* Update dependency js-yaml to v4.3.0 [SECURITY] by @renovate[bot] in https://github.com/next-theme/hexo-theme-next/pull/979
* Update dependency mocha to v11.8.0 by @renovate[bot] in https://github.com/next-theme/hexo-theme-next/pull/980
* Update Netlify logo URL (333b2c7)

**Full Changelog**: https://github.com/next-theme/hexo-theme-next/compare/v8.28.0...v8.29.0

[Detailed changes for NexT v8.29.0](https://github.com/next-theme/hexo-theme-next/releases/tag/v8.29.0)
