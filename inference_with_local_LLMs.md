# Introduction

This article is about using a pre-trained 'open' Large Language Model (LLM) for **inference**, asking it to process your input text, on your local computer. It may not be as fancy as some of the latest online models and services, but it will be free of charge for as much usage as you want, and importantly your confidential data will never leave your computer.

This article uses the Ollama wrapper and suggests Meta's Llama model as a good starting point, but there are many more options out there once you get up to speed. If Meta start withholding their model, to commercialise it, you will likely find other models rapidly become popular among the 'open' user community. 

## About Models and more

Behind chatbots like ChatGPT or Bard are the (large language) *Models* (e.g. OpenAI's GPT-4 or Google's PaLM2). Some of these are kept behind closed doors, but there are many that are openly downloadable (such as Llama). 

In natural language processing (NLP), machine-learning Models store partial words as 'embeddings', which are related in a particular 'context space'. The model is (pre-)*Trained* and then fine-tuned, giving weightings to the embeddings, and these 'hard weights' inside are how they remain. 

A trained model can be used directly for *Inference*, the process of running live data through it – evaluating weights to see if you get 'activations' (think neuron firing) – all this leads to an output: perhaps to evaluate, to predict or to solve a task. The costs of storage volumes and computation time and energy can be traded down by using *Quantisation* – using much less precise numbers for the weights and calculating activations. 

However, *Attention* gives a mechanism for adding 'soft weights' to a model. *Transformers* are a way to use 'parallel multi-headed attention mechanisms' that affect the way that a trained model evaluates the input data. This is a way to specialise a model even further than its initial fine-tuning.

Transformers are typically built using a *Framework* – PyTorch grew out of research, and kept going beyond TensorFlow, pushed by Google a few years back – who are now building up JAX as a successor. Of course frameworks are used during training too, but you are not obliged to use the same one for inference – some people chop and change according to circumstances. 

Weights?

For chat models, based on a conversation-like stream of messages, a final layer of tweaking can come from *Templates*.  

## About Ollama

Ollama is a simple GoLang wrapper around the `llama.cpp` project. Llama.cpp is an open-source C++ tool for running inference on LLaMA models, that has become the base for many other projects. Although its focus is optimal execution on Apple silicon and Mac hardware, and it continues to evolve allowing GPU backend support, and a range of quantization precisions to enhance efficiency.

# Install Ollama

To be able to run local instances of the LLaMA family of open source Large Language Models from Meta on macOS:

```zsh
brew install ollama
```

You could use the Docker Image in Docker Desktop for a more esoteric configuration, but installing the desktop app get you going quickly and easily.

To modify [where Ollama stores it's models](https://github.com/jmorganca/ollama/blob/main/docs/faq.md#where-are-models-stored)

```
export OLLAMA_MODELS="$HOME/Cached/Oversize/Ollama"
mkdir -p $OLLAMA_MODELS
```
And add this to your env
```zsh
cat >> ${ZDOTDIR}/.zshenv <<EndOfConfigZshEnv

# move the location for the very large (and replaceable) Ollama model data

export OLLAMA_MODELS="\$HOME/Cached/Oversize/Ollama"

EndOfConfigZshEnv
```

# Start Ollama

## Server

you can just run this in a terminal tab
```zsh
ollama serve
```
and then leave it running so you can see what happens. You can open another terminal tab to run other commands. 

If you want the server to start automatically every time you start up your device, use
```zsh
brew services start ollama
```

## Download and run model

See which models you already have locally...
```zsh
ollama list
```

To understand the relationship between Llama2 models see the [diagram explaining Code Llama](https://llama.meta.com/get-started/#code-llama). On the left, the basic pre-trained model is suffixed `-text` and the one without the suffix is Chat.

* Pick one according to your memory from https://ollama.com/library/llama3.1 or https://ollama.ai/library/codellama 
* Be conservative with your choice, otherwise your model performance will suffer, and you will end up inefficiently swapping out to your ssd, which won't thank you in the long run.
* If you are concerned about memory or performance, try `llama3.2` which is a small (3B) model
* downloading 4 GB (7b) or 8 GB for the below may take a while on many connections

e.g. 
```zsh
ollama run llama3.1
```

# Use Ollama

## Integrate with IDE

```zsh
# install your development environment (code editor) 

brew install visual-studio-code 

code --install-extension continue.continue 
```


## Tips

### Prompting tips

* Meta would know best, so: https://llama.meta.com/get-started/#prompting
* https://zdwolfe.medium.com/a-weekend-with-code-llama-f6a964f4b92f
* 

### Recipes

* https://github.com/meta-llama/llama-recipes/tree/main/recipes
* 

### Troubleshooting

* https://continue.dev/docs/troubleshooting
* 

### Bonus

See how to use Llama on Obsidian notes: https://ollama.ai/blog/llms-in-obsidian


## Advanced

See:
* examples of a Model File to create a new model including Template layers.
	* https://www.markhneedham.com/blog/2023/10/11/ollama-few-shot-prompting-experiments-llama2-7b/
* 

