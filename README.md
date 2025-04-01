# git-bob_test repo
Original workflow, work and repository [https://github.com/haesleinhuepf/git-bob](https://github.com/haesleinhuepf/git-bob) by [https://github.com/haesleinhuepf](https://github.com/haesleinhuepf). This is just a very simple testing repo, read and study the source if you want to learn more.
## Origin of git-bob ![](logo_32x32.png)
[![PyPI](https://img.shields.io/pypi/v/git-bob.svg?color=green)](https://pypi.org/project/git-bob)
[![codecov](https://codecov.io/gh/haesleinhuepf/git-bob/branch/main/graph/badge.svg)](https://codecov.io/gh/haesleinhuepf/git-bob)
[![DOI](https://zenodo.org/badge/831841421.svg)](https://doi.org/10.5281/zenodo.13970719)
[![License](https://img.shields.io/pypi/l/git-bob.svg?color=green)](https://github.com/haesleinhuepf/git-bob/raw/main/LICENSE)
<!--[![PyPI - Downloads](https://img.shields.io/pypi/dm/git-bob)](https://pypistats.org/packages/git-bob)-->

git-bob uses AI to solve GitHub issues. It runs inside the GitHub CI, no need to install anything on your computer.
Read more in the [publication](https://www.nature.com/articles/s43588-025-00781-1). 

![banner](https://github.com/haesleinhuepf/git-bob/raw/main/docs/images/banner2.png)

Under the hood it uses [Anthropic's Claude](https://www.anthropic.com/api) or [OpenAI's chatGPT](https://openai.com/blog/openai-api) or [Google's Gemini](https://blog.google/technology/ai/google-gemini-ai/) to understand the text and 
[pygithub](https://github.com/PyGithub/PyGithub) to interact with the issues and pull requests. As its discussions are conserved, you can document how things were done using AI and 
others can learn how to prompt for the things you did. For example, the pair-plot discussion above is [available online](https://github.com/haesleinhuepf/git-bob-playground/issues/48).

## Disclaimer

`git-bob` is a research project aiming at streamlining GitHub interaction in software development projects. Under the hood it uses
artificial intelligence / large language models to generate text and code fulfilling the user's requests. 
Users are responsible to verify the generated code according to good scientific practice.

When using `git-bob` you configure it to use an API key to access the AI models. 
You have to pay for the usage and must be careful in using the software.
Do not use this technology if you are not aware of the costs and consequences.

> [!CAUTION]
> When using the Anthropic, OpenAI, Google Gemini, Mistral or any other endpoint via git-bob, you are bound to the terms of service 
> of the respective companies or organizations.
> The GitHub issues, pull requests and messages you enter are transferred to their servers and may be processed and stored there. 
> Make sure to not submit any sensitive, confidential or personal data. Also using these services may cost money.

## Installation as GitHub action

There is a detailed [tutorial](https://github.com/haesleinhuepf/git-bob/blob/main/docs/installation-tutorial.md) on how to install git-bob as GitHub action to your repository. In very short, to use git-bob in your GitHub repository, you need to 
* Copy the [git-bob](https://github.com/haesleinhuepf/git-bob/blob/main/.github/workflows/git-bob.yml) GitHub workflow in folder `.github/workflows/` to your repository.
  * Make sure to replace `pip install -e .` with a specific git-bob version such as `pip install git-bob==0.16.0`.
  * If your project does not contain a `requirements.txt` file, remove the line `pip install -r requirements.txt`.
  * Configure the LLM you want to use in the workflow files by specifying the `GIT_BOB_LLM_NAME` secret. These were tested:
    * `claude-3-5-sonnet-20241022`
    * `gpt-4o-2024-08-06` - **tested in this repo**
    * `gpt-4o`
    * `meta-llama-3.1-405b-instruct`
    * `gemini-1.5-pro-002` - **tested in this repo**
    * `mistral-large-2411` (uses `pixtral-12b-2409` for vision tasks)
    * `deepseek-chat`
  * configure a GitHub secret with the corresponding key from the LLM provider depending on the above configured LLM:
    * `OPENAI_API_KEY`: [OpenAI (gpt)](https://openai.com/blog/openai-api)
    * `ANTHROPIC_API_KEY`: [Anthropic (claude)](https://www.anthropic.com/api)
    * `GH_MODELS_API_KEY`: [GitHub Models Marketplace](https://github.com/marketplace/models)
    * `GOOGLE_API_KEY`: [Google AI](https://ai.google.dev/gemini-api/docs/api-key)
    * `MISTRAL_API_KEY`: [Mistral](https://console.mistral.ai/api-keys/)
    * `DEEPSEEK_API_KEY`: [DeepSeek](https://platform.deepseek.com/api_keys)
    * `KISSKI_API_KEY`: [KISSKI](https://services.kisski.de/services/en/service/?service=2-02-llm-service.json)
    * `BLABLADOR_API_KEY`: [BLABLADOR](https://login.helmholtz.de/oauth2-as/oauth2-authz-web-entry)
  * configure GitHub actions to run the workflow on issues and pull requests. Also give write-access to the Workflow using the `GITHUB_TOKEN`.

When using it in your repository, you can also set a custom system message, for example for:
* [General Data Science / Python Programming](https://github.com/haesleinhuepf/git-bob-playground/blob/bf08b3526980e011f632c13f29ae65372aafa5c7/.github/workflows/git-bob.yml#L75)
* [Bio-Image Analysis](https://github.com/haesleinhuepf/git-bob-bioimage-analysis-example/blob/main/.github/workflows/git-bob.yml#L75)
* [Giving advice on a specific repository / library](https://github.com/haesleinhuepf/stackview/blob/afc662a71a39f298af9f183c06c3d37c95cc2015/.github/workflows/git-bob.yml#L58)
* [Manuscript writing](https://github.com/haesleinhuepf/git-bob-manuscript/blob/49659f8a41854d4da696259e7c1316af2fc7c171/.github/workflows/comment-on-issue.yml#L49)

Furthermore, to guide discussions, you may want to setup issue templates, e.g.
* [General Python Programming Questions](https://github.com/haesleinhuepf/git-bob-playground/blob/main/.github/ISSUE_TEMPLATE/programming.md)
* [Bio-Image Analysis](https://github.com/haesleinhuepf/git-bob-playground/blob/main/.github/ISSUE_TEMPLATE/bioimage_analysis.md)
* [Statistics and Plotting](https://github.com/haesleinhuepf/git-bob-playground/blob/main/.github/ISSUE_TEMPLATE/statistics_plotting.md)

## Usage: Trigger words

To trigger git-bob, you need to comment on an issue or pull request with the `comment` trigger word (or aliases `think about`, `review`, `respond`):

```
git-bob comment
```

If you want to ask git-bob for a review of a pull-request, you can use the `review` trigger word. Also make sure mention explictly what you want to be reviewed.

```
git-bob review this PR. Check code quality and comments.
```

After some back-and-forth discussion, you can also use the `solve` trigger word (or aliases `implement`, `apply`) make git-bob solve an issue and send a pull-request. 
This trigger can also be used to modify code in pull requests.

```
git-bob solve
```

You can ask git-bob to implement a solution for testing, without sending a pull-request, using the `try` trigger:
```
git-bob try
```

If you have multiple API-Key for different LLMs configured, you can specify the LLM in the command using the `ask <LLM-Name> to` trigger command:

```
git-bob ask claude-3-5-sonnet-20241022 to solve this issue.
```

If the issue is complex and should be split into sub-issues, you can use the following command:

```
git-bob split
```

If you have two GitHub secrets `TWINE_USERNAME` and `TWINE_PASSWORD` configured, you can also use the following command to publish a new version of your library to PyPI:

```
git-bob deploy
```

All trigger words can be combined with `please` and/or `,`, which will make no difference to calling git-bob without these words:

```
git-bob, please ask gemini-1.5-pro-002 to solve this issue.
```

