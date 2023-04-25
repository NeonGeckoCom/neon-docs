---
description: Template for the Code Review component of the Skills Acceptance Process.
---

# Code Review Template

```text
## Meta

* Platform:  
* Mycroft-core version:  
* Who: [@your-github-handle](https://github.com/your-github-handle) / @your-mycroft-chat-handle  
* Datestamp: run `date +%Y-%m-%d_%H:%M:%S_%Z`  
* Language and dialect of tester:

## 0. Automated tests

Are all automated tests passing?

* [ ] Skill tester - Jenkins  
* [ ] Continuous Integration - Travis-CI

## 1. Code Review - secure and stable

* [ ] **Code Quality**  

  Can you understand what the code is doing? Is there inline documentation? Do you have any concerns about this code running on your machine? Are there any performance issues such as nested or infinite loops? Do you have significant concerns about the overall code quality?  

  _NOTE: We do not enforce PEP8 Checks on Skills_

&gt;

* [ ] **Error Handling**  

  Are there any specific checks we make for error handling or graceful degradation?

&gt;

* [ ] **Libraries**  

  Does the Skill include the correct libraries? Does it use too many libraries or dependencies?

&gt;

* [ ] **Required Dependencies**  

  Check `requirements.txt` and `requirements.sh` - are the required dependencies listed? If `requirements.sh` is used, is some form of conditional processing done to match against multiple distros? Often Skill Authors will add requirements.txt using only an “library=1.x.x” instead of “library &gt;=1.x.x”. Check to make sure that there is an equal or greater than in the requirements to help future-proof the Skill, unless a _specific_ version is needed.

&gt;

* [ ] **Settings** Is the `settingsmeta` file well laid out? If settings are not used, has the default file been deleted? If it is the default file, the first setting section will be called "Options &lt;&lt; Name of section". &gt;
* [ ] **Integration Tests** Does the skill include sufficient integration tests, included in the `test` folder?

&gt;

* [ ] **Other Files**  

  Are there any other files included that are unnecessary or you are unsure of their function?

&gt;

### Actions Required:

\*
```

