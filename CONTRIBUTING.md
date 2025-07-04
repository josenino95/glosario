Thank you for your interest in contributing to Glosario! 
We welcome contributions of all kinds.
If you are here to submit a definition,
either for a new term
or in another language for an existing term,
please take a moment to read the guidance below.

## Contribution Workflow

When you are making your contribution(s) to Glosario, please adhere to the following guidance:

1. **check the [open Issues][issues] and [Pull Requests][pulls]** to see what terms and definitions other contributors are already working on. This will help you avoid accidentally spending time writing the same term(s)/definition(s) as someone else.
2. **add one or a small number of related new terms or definitions per Pull Request.** We woud love to receive multiple contributions from you but please keep each new term or set of terms and definitions in a distinct Pull Request. This makes it much easier for the Glosario maintainers/editors to review your contributions. By [creating a new branch][github-branches] from the `master` branch and making a new Pull Request each time, you also reduce the chance of accidentally combining multiple contributions.
3. If you plan to contribute a larger number of terms e.g. to cover a new domain, **[open an Issue][issues]** with a title in the format "[language]/domain" e.g. "Romanian/relational database terms." This will help others avoid devoting time to writing the same terms/definitions as you.

In the sections below, we detail common ways you can contribute to glosario.

## Adding a new term to the glossary

To add a new entry, please [fork][forking-guide] the [main Glosario repository][repo] and, on a new branch, add the term and definition to [`glossary.yml`][glossary]. This glossary file is written in [YAML]. If you already have a fork of the repository, then please [make sure your fork is up-to-date](https://happygitwithr.com/upstream-changes.html#upstream-changes) with the [main Glosario repository][repo].

When adding a new term or translation, please take care with the indentation on the YAML file. Indentation is syntactically significant in YAML.

In case you wish to build up the website locally to double check the final look, you can use `make serve`. 

Here is an example of how your glossary entry should be structured:

```
- slug: cran
  ref:
    - base_r
    - tidyverse
  en:
    term: "Comprehensive R Archive Network"
    acronym: "CRAN"
    def: >
      A public repository of R [packages](#package).
```

-   The value associated with the `slug` key identifies the entry.
    -   It must be unique within the glossary.
    -   It must be in lower case and use only letters, digits, and the underscore
        (to be compatible with Jekyll's automatic slug creation).
-   The entry *may* have a `ref` key.
    If it is present,
    its value must be a list of identifiers of related terms in this glossary.
-   Every other top-level key must be a two-letter ISO 639 language code such as `en` or `fr`.
    (Refer to the "639-1" column of [this table][iso639-table-en].)
    -   Every entry must have at least one such language section.
-   Within each language section for each term:
    -   The value of `term` is the term being defined.
        This key must be present.
    -   The key `acronym` is optional.
        If present, its value is the acronym for this term.
    -   The value of `def` is the definition.
        This key must be present,
        and the value may contain local links to other terms in this glossary
        (i.e., links starting with `#`)
        and/or links to outside sources.
    -   Note that, regardless of the language of their associated values, the keys like `term`, `acronym`, and `def` are not to be translated.

Once your term and definition(s) are complete, [make a Pull Request][pr-guide] back to the main repository.

## Adding a definition in another language to an existing term

To add a new definition to an existing term in the glossary, please fork [the main Glosario repository][repo] and, on a new branch, find the term in [`glossary.yml`][glossary] and add the two-letter ISO 639 language code such as `en` or `fr` as a new top-level key. (Refer to [the "639-1" column of this table][iso639-table-en].)

You can then fill in the `term`, `def`, and (if appropriate) `acronym` for that term in your chosen language. The example below shows a term entry with definitions in English (`en`), Spanish (`es`), and French (`fr`):

```
- slug: global_variable
  ref:
    - local_variable
  en:
    term: "global variable"
    def: >
      A variable defined outside any particular function, which is therefore visible
      to all functions.
  es:
    term: "variable global"
    def: >
      Una variable definida fuera de alguna función en particular, por lo que es visible
      para todas las funciones.
  fr:
    term: "variable globale"
    def: >
      Une variable définie en dehors d'une fonction donnée, qui est par conséquent visible pour
      toutes les fonctions.
```

## Adding a new language

If you want to add a term or define a term in a new language, you can quickly check if your language is defined in glosario by heading to `https://glosario.carpentries.org/[lang]`, replacing `[lang]` with the [two-letter ISO 639 language code][iso639-table-en]. If you see a 
[page not found error][https://glosario.carpentries.org/not-found], then that means you are the first to add this language to glosario!

To add your language, first follow the instructions for [adding a new term to the glossary](#adding-a-new-term-to-the-glossary) or [adding a definition in another language to an existing term](#adding-a-definition-in-another-language-to-an-existing-term). Next, you will:

1. create a landing page for your language;
2. add the entry for the language in the `_config.yml` file
3. add the 2-letter code among the list of languages that are recognized when
   checking the content of the glosary.

To see an example of the changes required, you can look at the [Pull
Request](https://github.com/carpentries/glosario/pull/313/files) that adds the
required infrastructure to support entries in Italian.

### 1. Create the language landing page

To add a landing page for your language, create a new markdown file that is named with the two-letter ISO 639 code for your language. For example, if you wanted to add support for Italian, you would create a new markdown file called `it.md`. 

Each markdown file requires two things: a yaml header that defines the permalink to the page and a Jekyll includes statment in the body. For example, if you wanted to create an entry for Italian, you would add `it.md` with the following content:

```markdown
---
permalink: /it/
layout: glossary
direction: ltr
---

{% include glossary.html %}
```

The `layout` yaml item specifies the template for the landing page and the 
`direction` yaml item is used to indicate the direction the language is read in:
- for left-to-right (ltr) languages, use `direction: ltr`;
- for right-to-left (rtl) languages, use `direction: rtl`.

### 2. Add the language entry in the `_config.yml` file

For the language to appear on the main landing page for the site, add the
following to the `_config.yml` file under the `language:` entry, respecting the alphabetical order of the two-letter ISO 639 code:

```markdown
  - key: { Two-letter code }
    name: { Name of the language }
    title: { Translation of "glossary" into the language }
    blurb: >
       { Translation of the blurb presenting the glosario project}
```

### 3. Add the two-letter code to the script checking the content of the glossary

To make sure that entries in the glossary for the language added will be recognized, you need to add the two-letter ISO code for the language in the Python script [`utils/check-glossary.py`](https://github.com/carpentries/glosario/blob/main/utils/check-glossary.py#L33):

```python
ENTRY_LANGUAGE_KEYS = {'af', 'am', 'ar', 'bn', 'de', 'en', 'es', 'fr', 'he', 'it', 'ja', 'nl', 'pt', 'zu'}
```



[forking-guide]: https://guides.github.com/activities/forking/
[github-branches]: https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/managing-branches
[glossary]: https://github.com/carpentries/glosario/blob/master/glossary.yml
[iso639-table-en]: https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
[issues]: https://github.com/carpentries/glosario/issues
[new issue]: https://github.com/carpentries/glosario/issues/new
[pr-guide]: https://guides.github.com/activities/forking/#making-a-pull-request
[pulls]: https://github.com/carpentries/glosario/pulls
[repo]: https://github.com/carpentries/glosario
[yaml]: https://learnxinyminutes.com/docs/yaml/
