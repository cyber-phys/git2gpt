# git2gpt
[Mirror of git2gpt](https://github.com/karnagraha/git2gpt)

This repository is an experiment at bootstrapping prompt-driven self-modifying software with GPT-4.

## Basic features:

- Provides snapshot of a git repo to the GPT-4 API.
- Prompt GPT-4 to generate modifications to the repo.
- Review and apply changes generated by the GPT-4 API.

Note that while this was initially an experiment in self-modifying code, git2gpt will generate prompt-driven changes for any repo you lying around, which you can specify with the --repo flag, though be aware of the context size limits you have available.

## Usage:

1. Run `pip install git2gpt`
2. Set up your OPENAI_API_KEY.  Either write it to your ~/.env like this: `echo 'OPENAI_API_KEY="sk-<key>" >> ~/.env` or set it in your environment `export OPENAI_API_KEY=sk_key`
3. Change into a repo and make your first change `cd somerepo ; git2gpt --prompt "add Hello World to the README"`

## Contributing

0. Optionally: create and activate a virtualenv
   `python3 -m venv env`
   `. env/bin/activate`
1. Install the required packages using pip:
   `pip install -r requirements.txt`
2. Set up your OpenAI API key:
   `echo "OPENAI_API_KEY=your_key_here" > .env`
3. Run the main script:
   `python main.py --prompt <prompt to modify the code>`
4. Or, install the package in editable mode `pip install -e .` and use the `git2gpt` command.


Instead of specifying the prompt on the commandline, you can also call `git2gpt --editor` to open your default $EDITOR to use while crafting the prompt.


## Building and Uploading Package

After making any changes to the code, you can build the package and upload it to PyPI by running the provided script:

```
chmod +x build_and_upload.sh
./build_and_upload.sh
```

Make sure you have the `twine` package installed and configured with your PyPI credentials.

### Building

Building git2gpt using ChatGPT Plus was very straightforward:

1. I pasted in some relevant documentation (like the OpenAI Chat Completions API) and chatted a bit about what I wanted to build.
2. I then prompted it to write code that translates a git archive tar to a json format. This code now lives in `git2gpt/git_to_json.py`
3. Using this code, I iterated using the ChatGPT Plus UI by pasting a full json snapshot of the current state of the repository, and prompting it to generate various changes, then applying those changes.

As part of the experiment I tried to avoid all manual changes, though ultimately did some tuning of the prompt-generating code at the very end.

### Results

Building this in conjunction with GPT-4 was easy, but a little tedious. This library should help reduce the tedium of this kind of workflow in the future. It's not ready to replace programmers quite yet; one definitely still needs to have an idea what to prompt it to build, and to be able to evaluate the result for fitness to purppose.

I encourage experimenting with writing software using this kind of workflow, I think a lot of software development might start looking like this in the future.

## Limitations:

- Obviously, you should review all changes before running any code from third parties, including from GPT-4.
- I would say the generation quality seemed to get worse as my repo grew larger. I plan to experiment and tune the process once I get access to the GPT-4 API.
