# Claude with Google Vertex AI - Course Summary

**Course URL:** https://anthropic.skilljar.com/claude-with-google-vertex

---

## 🎯 Course Overview

Deploy and use Claude models via Google Cloud Vertex AI. Covers authentication, API access, model selection, prompt engineering, tool use, and building AI workflows on Google Cloud infrastructure.

---

## 📚 Table of Contents


**Introduction**
  📌 Welcome to the course

**Anthropic overview**
  📌 Overview of Claude models

**Accessing Claude with the API**
  📌 Accessing the API
  📌 Vertex AI Setup
  📌 Making a request
  📌 Multi-turn conversations
  📌 Chat exercise
  📌 System prompts
  📌 System prompts exercise
  📌 Temperature
  📌 Response streaming
  📌 Structured data

**Prompt evaluation**
  📌 Prompt evaluation
  📌 A typical eval workflow
  📌 Generating test datasets
  📌 Running the eval
  📌 Model based grading
  📌 Code based grading

**Prompt engineering techniques**
  📌 Being clear and direct
  📌 Introducing tool use
  📌 Project overview
  📌 Tool functions
  📌 Tool schemas
  📌 Handling message blocks
  📌 Sending tool results
  📌 Multi-turn conversations with tools
  📌 Implementing multiple turns
  📌 Using multiple tools
  📌 The batch tool
  📌 Tools for structured data
  📌 The text edit tool
  📌 The web search tool
  📌 Introducing Retrieval Augmented Generation
  📌 Text chunking strategies
  📌 Text embeddings
  📌 The full RAG flow
  📌 Implementing the RAG flow
  📌 BM25 lexical search
  📌 A Multi-index RAG pipeline
  📌 Reranking results
  📌 Contextual retrieval
  📌 Extended thinking
  📌 Image support
  📌 PDF support
  📌 Citations
  📌 Prompt caching
  📌 Rules of prompt caching
  📌 Prompt caching in action
  📌 Introducing MCP
  📌 MCP clients
  📌 Project setup
  📌 Defining tools with MCP

**Model Context Protocol**
  📌 The server inspector
  📌 Implementing a client
  📌 Defining resources
  📌 Accessing resources
  📌 Defining prompts
  📌 Prompts in the client
  📌 MCP review

**Anthropic apps - Claude Code and computer use**
  📌 Claude Code setup
  📌 Claude Code in action
  📌 Enhancements with MCP servers
  📌 Parallelizing Claude Code
  📌 Automated debugging
  📌 Computer use
  📌 How computer use works

**Agents and workflows**
  📌 Agents and workflows
  📌 Parallelization workflows
  📌 Chaining workflows
  📌 Routing workflows
  📌 Agents and tools
  📌 Environment inspection
  📌 Workflows vs agents
  ✅ Quiz on agents and workflows

**Final assessment**
  ✅ Final assessment quiz

**Wrapping up!**
  📌 Course Wrap Up

---

## 📖 Lesson Content


### 📖 Introduction

#### 1. Welcome to the course

Video-only lesson. Video: 01 - 001 - Welcome to the Course.mp4 (2 min 8 sec). No text content below video.


### 📖 Anthropic overview

#### 2. Overview of Claude models

Video-only lesson. Video: 02 - 001 - Overview of Claude Models.mp4 (3 min 56 sec). No text content below video.


### 📖 Accessing Claude with the API

#### 3. Accessing the API

When building applications with Claude, understanding the complete request lifecycle helps you architect better systems and debug issues more effectively. Let's walk through what happens when a user sends a message to your AI-powered chat application.

The Complete Request Flow

The journey from user input to AI response involves five distinct steps: Request to Server, Request to Vertex, Model Processing, Response to Server, and Response to Client. Each step plays a crucial role in delivering that "magical" response users expect.
Why You Need a Server

Never make API requests directly from client-side code. Here's why:

API requests require secret credentials that must stay secure
Exposing credentials in client code makes them visible to anyone
Your server acts as a secure intermediary between your app and Vertex

Always route requests through your own server that you control and secure.
Making the API Request

Your server communicates with Vertex using either Anthropic's SDKs or Google's official Vertex SDKs. Anthropic provides official SDKs for Python, TypeScript, Go, and Ruby.
Every request must include these key fields:

API Key - Identifies your request to Anthropic
Model - Name of the specific model to use
Messages - List containing the user's input text
Max Tokens - Limits how many tokens the model can generate

The user's input gets placed inside a "user" message, which then goes into a list of messages sent to the API.
Inside Claude: Text Generation Process

Once Vertex receives your request, Claude processes it through four stages: Tokenization, Embedding, Contextualization, and Generation.

Tokenization

Claude first breaks down the input text into smaller chunks called tokens. These can be whole words, parts of words, spaces, or symbols. For simplicity, think of each word as one token.
Embedding

Each token gets converted into an embedding - a long list of numbers that represents all possible meanings of that word. Think of embeddings as number-based definitions.
Contextualization

Since words can have multiple meanings, Claude uses context to determine the right interpretation. The word "quantum" could refer to physics, computing, or just mean "very small" - context from surrounding words clarifies the intended meaning.
During contextualization, each embedding gets adjusted based on its neighbors, highlighting the meaning that makes most sense given the context.
Generation


> *(See full lesson at course URL)*

#### 4. Vertex AI Setup

In the next video we will be making a request to Vertex AI in order to call a Claude model. To do so, you need to go through a little bit of setup.

Step One: Ensure Anthropic models are enabled in Vertex
In your browser, navigate to https://console.cloud.google.com/vertex-ai/dashboard
In the left hand nav, click on 'Model Garden'

In the 'Search models' box, enter 'Anthropic'
Click on the model that you want to use.
Step Two: Enable the Model
Once you've found the model you want to use, you may need to enable it. On the model information page, click the 'Enable' button
If you don't see an 'Enable' button then you already have access to the model

Step Three: Install the gcloud CLI

If you don't already have the gcloud CLI installed, follow the directions here to install and authenticate with the CLI: https://cloud.google.com/sdk/docs/install
Step Four: Login and set up authentication with the gcloud CLI

If you have not already logged in to the gcloud CLI, do so by running:

gcloud init
gcloud auth login

Then, set your project ID and set your default credentials:

gcloud config set project YOUR_PROJECT_ID
gcloud auth application-default login

That's it! The Anthropic SDK will automatically use these credentials when attempting to access Vertex.

#### 5. Making a request

Now that you've set up access to Vertex AI, let's make your first request to Claude. In this video, we'll walk through how to create a simple chat function that sends a message and receives a response.

Creating the Chat Function

Every AI-powered application starts with some variation of a chat function. Here's the basic pattern we'll build:
The function takes messages, sends them to Claude, and returns Claude's response. Simple but powerful.
Configuring Credentials

When using Vertex AI, you need to properly configure your credentials. Here's the setup:


First, you authenticate with Google Cloud using the gcloud CLI. Then the Anthropic SDK automatically picks up those credentials through Application Default Credentials.
The Code

Let's look at the actual implementation. We'll create a chat function that takes a message, sends it to Claude through Vertex, and returns the response.
The key parameters you need are:

- model: Which Claude model to use
- max_tokens: Maximum tokens in the response
- messages: The conversation history

Vertex requires additional configuration like project_id and region, but the core interaction with Claude remains the same.
Testing Your Setup


Once you've entered your code, try sending a message. If everything is configured correctly, you should receive a response from Claude. Common issues include authentication problems or incorrect parameter values.
If you encounter errors, double-check that you:
- Logged into gcloud CLI
- Set the correct project ID
- Enabled the Claude model in Vertex AI Model Garden

Making your first successful request is a major milestone - you've now got a working connection to Claude through Vertex AI!

#### 6. Multi-turn conversations

Claude excels at multi-turn conversations, where context from previous messages helps generate more relevant and coherent responses. In this lesson, we'll explore how to build conversational interfaces that maintain context across multiple exchanges.

The Conversation Pattern

Multi-turn conversations work by sending the entire conversation history with each request. Claude reads all previous messages to understand the full context of the current message.
How It Works

When you send a new message, you append it to the existing messages list. This list contains all the prior turns - both user messages and Claude's responses. Claude processes this complete history to generate its next response.

This is fundamentally different from single-turn interactions. Each message in the list represents a complete turn in the conversation.

Message Roles

The messages list contains objects with two key fields:
- role: Who is speaking - "user" or "assistant" 
- content: The actual text content

User messages contain the human's input. Assistant messages contain Claude's responses from previous turns.
Building the Messages List


The pattern is straightforward: start with an empty list, add user messages with role="user", and add assistant messages with role="assistant" after Claude responds.
The function add_user_message appends a new user message to the list. The function add_assistant_message appends Claude's response.
Conversation Memory

A critical aspect of multi-turn conversations is that you're building up the complete conversation history. This history becomes part of every subsequent request.

With each turn, the messages list grows longer. Claude considers everything that was said before when generating its next response. This allows for coherent, contextual conversations.
Practical Considerations


There are some practical aspects to consider:

First, the conversation history grows with each turn, which increases token usage and processing time.

Second, you need to manage the maximum context window - Claude can only process so many tokens at once. Very long conversations may require truncating or summarizing older messages.
Third, the order matters. Messages should be in chronological order from earliest to latest.
Testing the Pattern


> *(See full lesson at course URL)*

#### 7. Chat exercise

This lesson contains an exercise for practicing multi-turn conversations with Claude.

Exercise: Build a Conversational Chat Function

In this exercise, you'll create a Python chat application that maintains conversation history across multiple turns.

Setup

First, configure your Vertex AI credentials. Make sure you've logged into gcloud and set your default project.
Creating the Helper Functions

You'll need three helper functions:

add_user_message(messages, text) - Adds a user message to the messages list
add_assistant_message(messages, text) - Adds an assistant message to the messages list  
chat(messages) - Sends messages to Claude and returns the response

The chat function should use the anthropic.messages.create method to send your messages to Claude through Vertex AI.
Testing Your Implementation

Once you've built these functions, test them with a multi-turn conversation:

Turn 1: Ask Claude about its favorite type of music
Turn 2: Ask a follow-up question about the genre it mentioned
Turn 3: Ask Claude to recommend a song from that genre

Claude should demonstrate understanding of the conversation flow, referencing your previous messages when responding to follow-up questions.
Key Concepts to Verify


Your implementation should:
- Maintain a growing messages list
- Send the complete history with each request
- Properly format messages with role and content fields
- Handle Claude's responses and add them to the conversation

This exercise reinforces how multi-turn conversations work by building the pattern from scratch.

#### 8. System prompts

System prompts are one of the most powerful tools for customizing Claude's behavior. While user messages represent what humans say and assistant messages represent Claude's responses, the system prompt acts as foundational instructions that shape Claude's entire approach to every request.


Understanding the System Prompt

Think of the system prompt as instructions you give Claude before it starts any conversation. These instructions become part of every subsequent request, permanently influencing how Claude interprets and responds to user messages.

Unlike user messages which can vary from request to request, the system prompt stays constant. It tells Claude who it is, what its values are, and how it should behave in broad terms.
The System Parameter

In code, the system prompt is passed as a separate parameter when creating a message. Here's the structure:

The system parameter takes a string containing your instructions. These instructions apply to all requests using that chat function.

This differs from user messages which go into the messages list. The system prompt is separate, giving you clean separation between conversational content and behavioral instructions.
What to Include in System Prompts


System prompts work best when they define:


Claude's identity and role - Who should Claude be in this conversation?
Behavioral guidelines - How should Claude approach its responses?
Format expectations - What structure should responses follow?

For example, a system prompt might tell Claude to act as a Python tutor who explains concepts in simple terms, includes code examples, and always verifies understanding before moving on.
Combining with User Messages

User messages work alongside system prompts. The system prompt provides overall direction while user messages provide specific inputs for each turn.
Claude reads both when generating responses. The system prompt influences the broad approach while user messages contain the immediate task or question.
Best Practices


Keep system prompts clear and specific. The more precisely you define Claude's role and expectations, the more consistently Claude will behave as intended.

Avoid contradictory instructions in your system prompt and user messages. If conflicts arise, Claude may become confused about which instruction to follow.
Test your system prompts with various inputs to ensure they produce the behavior you expect across different scenarios.

#### 9. System prompts exercise

This lesson contains an exercise for practicing system prompts with Claude.


Exercise: Customize Claude's Behavior with System Prompts

In this exercise, you'll use system prompts to customize how Claude responds to user messages.

Setup

Start with the chat function you built in previous exercises. Make sure it accepts a system parameter alongside messages.
Creating the System Prompt

Design a system prompt that customizes Claude to serve as a helpful Python code reviewer. Your system prompt should instruct Claude to:


Review Python code for potential bugs and issues
Suggest improvements that follow best practices
Explain problems in simple terms
Include corrected code examples when suggesting fixes

Testing Your Implementation

Once you've defined your system prompt, test it with a code review request. Send Claude a Python code snippet that contains a bug or poor practice.
Claude should respond as a code reviewer, identifying issues and suggesting improvements - not just as a general assistant.
Compare with Baseline


After testing with your custom system prompt, try the same code snippet without a system prompt. Notice the difference in how Claude approaches the task.

With the system prompt, Claude should focus specifically on code review. Without it, Claude might provide general commentary or answer different questions than expected.
Key Takeaways

This exercise demonstrates how system prompts fundamentally change Claude's behavior without changing the underlying model or user messages. The same input can produce very different outputs based on system prompt instructions.

#### 10. Temperature

Temperature is a powerful parameter that controls how predictable or creative Claude's responses will be. Understanding how to use it effectively can dramatically improve your AI applications.

How Claude Generates Text

Before diving into temperature, it's helpful to understand Claude's text generation process. When you send Claude a prompt like "What do you think?", it goes through three main steps:

Tokenization - Breaking your input into smaller chunks
Prediction - Calculating probabilities for possible next words
Sampling - Choosing a token based on those probabilities

In this example, Claude might assign a 30% probability to "about", 20% to "would", 10% to "of", and so on. The model then selects one token and repeats this process to build complete responses.
What Temperature Does


Temperature is a decimal value between 0 and 1 that directly influences these selection probabilities. It's like adjusting the "creativity dial" on Claude's responses.
At low temperatures (near 0), Claude becomes very deterministic - it almost always picks the highest probability token. At high temperatures (near 1), Claude distributes probability more evenly across options, leading to more varied and creative outputs.
Temperature Ranges and Use Cases


Different tasks call for different temperature settings:


Low Temperature (0.0 - 0.3)
- Factual responses
- Coding assistance
- Data extraction
- Content moderation

Medium Temperature (0.4 - 0.7)
- Summarization
- Educational content
- Problem-solving
- Creative writing with constraints

High Temperature (0.8 - 1.0)
- Brainstorming
- Creative writing
- Marketing content
- Joke generation
Implementing Temperature in Code

Adding temperature support to your chat function is straightforward. Here's how to modify your existing function:
The key changes are adding temperature as a parameter and including it in the params dictionary.
Testing Temperature Effects


To see temperature in action, try generating movie ideas with different settings:

With temperature=0.0, you might consistently get responses like "A time-traveling archaeologist must prevent ancient artifacts from being stolen." With temperature=1.0, you'll see much more variety in the creative concepts generated.
Key Takeaways


> *(See full lesson at course URL)*

#### 11. Response streaming

When building chat applications with Claude, there's a significant user experience challenge: responses can take 10-30 seconds to generate, leaving users staring at a loading spinner. The solution is response streaming, which lets users see text appear chunk by chunk as Claude generates it, creating a much more responsive feel.


The Problem with Standard Responses

In a typical chat setup, your server sends a user message to Claude and waits for the complete response before sending anything back to the client. This creates an awkward delay where users have no feedback that anything is happening.
How Streaming Works

With streaming enabled, Claude immediately sends back an initial response indicating it has received your request and is starting to generate text. Then you receive a series of events, each containing a small piece of the overall response.


Your server can forward these text chunks to your client application as they arrive, allowing users to see the response building up word by word. All of these events are part of a single request to Claude.
Understanding Stream Events

When you enable streaming, Claude sends back several types of events:


MessageStart - A new message is being sent
ContentBlockStart - Start of a new block containing text, tool use, or other content
ContentBlockDelta - Chunks of the actual generated text
ContentBlockStop - The current content block has been completed
MessageDelta - The current message is complete
MessageStop - End of information about the current message

The ContentBlockDelta events contain the actual generated text that you'll want to display to users.
Basic Streaming Implementation

To enable streaming, add stream=True to your messages.create call:
Rather than manually parsing events, you can use the SDK's simplified streaming interface that extracts just the text content:
This approach automatically filters out everything except the actual text content, which is usually what you need for displaying responses to users.
Getting the Final Message

While streaming is great for user experience, you often need the complete message for storage or further processing. After streaming completes, you can get the assembled final message:
This gives you both the streaming capability for user experience and the complete message object for database storage or conversation history.
Practical Considerations



> *(See full lesson at course URL)*

#### 13. Structured data

When you need Claude to generate structured data like JSON, Python code, or bulleted lists, you'll often run into a common problem: Claude wants to be helpful and add explanatory text around your content. While this is usually great, sometimes you need just the raw data with nothing else.


Consider building a web app that generates AWS EventBridge rules. Users enter a description, click generate, and expect to see clean JSON they can immediately copy and use. If Claude returns the JSON wrapped in markdown code blocks with explanatory headers and footers, users can't simply hit "copy all" - they'd have to manually select just the JSON portion.


This pattern shows up whenever you're generating structured data. Claude naturally wants to explain its work, but in many cases, you want only the content you're asking for and nothing else.

Combining Stop Sequences with Assistant Message Prefilling


The solution combines two techniques we've covered: stop sequences and assistant message prefilling. Here's how it works in practice:
When you run this code, you get back just the JSON content without any markdown formatting or additional commentary.
How It Works Behind the Scenes


Here's what happens when Claude processes your request:

Claude reads your user message and thinks "I need to write a full rule and probably describe it"
It sees the prefilled assistant message and assumes it already started writing the JSON markdown block
Claude thinks "Oh, I've already started the JSON part, so I just need to write the actual JSON content"
It generates the JSON content
When Claude tries to close the markdown block with "```", it hits the stop sequence and generation stops immediately

The result is that you get everything between the prefilled start and the stop sequence - exactly the content you wanted.
Cleaning Up the Output


The returned text might have some extra newlines, but you can easily clean this up by parsing as JSON to validate and format.

This technique works for any structured data format, not just JSON. Whether you're generating Python code, bulleted lists, or any other specific content format, you can use assistant message prefilling to start the response and stop sequences to end it exactly where you want.
This approach gives you precise control over Claude's output format, ensuring your applications get clean, usable data without extra formatting or commentary that might interfere with downstream processing.


### 📖 Prompt evaluation

#### 14. Prompt evaluation

In an earlier lesson, you learned how to build a chat function and make requests to Claude. But how do you know if your prompts are actually working well? This is where prompt evaluation comes in - a systematic approach to measuring and improving your prompts over time.


What is Prompt Evaluation?


Prompt evaluation is the practice of systematically testing your prompts against representative test cases to ensure they produce consistently good results. Rather than manually reviewing each response, you build automated pipelines that score outputs and track performance metrics.

Why Evaluation Matters


When you start building AI applications, initial prompt design is just the beginning. Users will interact with your prompts in unexpected ways, edge cases will arise, and you'll need to iterate on your designs. Without a solid evaluation system, you're essentially guessing whether your prompts are working.

Evaluation helps you:


Understand how well your prompt performs across diverse inputs
Catch regressions when you make changes
Make data-driven decisions about prompt improvements
Communicate quality to stakeholders
The Evaluation Workflow

Effective prompt evaluation follows a clear process:

1. Define what good output looks like for your use case
2. Create or generate a dataset of test cases
3. Run your prompt against each test case
4. Grade the outputs using automated graders
5. Analyze results and identify failure patterns
6. Iterate on your prompt to address issues
7. Repeat until you hit your quality targets

This process transforms prompt development from guesswork into science.
Key Concepts We'll Cover

In upcoming lessons, we'll dive deep into each part of the evaluation workflow:


A typical eval workflow - Understanding the complete cycle from test data to score
Generating test datasets - Creating representative samples for testing
Running the eval - Building the pipeline that processes test cases
Model based grading - Using AI to evaluate outputs
Code based grading - Programmatic evaluation of outputs
Exercises to practice these techniques

By the end of this section, you'll have a complete toolkit for evaluating and improving prompts systematically.

#### 15. A typical eval workflow

Now that you understand why prompt evaluation matters, let's walk through what a typical evaluation workflow looks like from start to finish.


The Five-Step Process

A complete evaluation workflow consists of five key steps that cycle through continuously as you improve your prompts.

Step 1: Define Your Eval Criteria

Before running any evaluation, you need to define what "good" looks like. This means establishing clear criteria that your prompt should satisfy. For a code generation prompt, you might care about:


Format - Does the output match the expected structure?
Correctness - Does the code actually work?
Completeness - Does it handle edge cases?
Readability - Is the code clean and understandable?

Different use cases have different criteria. Be specific about what you're measuring.
Step 2: Generate Test Data


Next, you need a dataset of test cases that represents the kinds of inputs your application will face. These test cases should be:

Representative - Cover the range of real inputs users will send
Sufficient - Large enough to give statistically meaningful results
Labeled - Ideally with expected outputs or scoring guidelines

Creating good test data is often the hardest part of evaluation. We'll cover strategies for this in an upcoming lesson.
Step 3: Build the Eval Pipeline

The eval pipeline is the code that connects everything together. It takes each test case, feeds it to your prompt, collects the output, and runs it through your grading logic.


A typical pipeline structure:
- Load the test dataset
- For each test case, merge with your prompt template
- Send to Claude and collect the response
- Run the output through your grader
- Store results for analysis
Step 4: Analyze Results

After running your pipeline, you'll have a collection of outputs and scores. Analysis involves:


Looking at aggregate scores (average, distribution)
Identifying specific failure cases
Understanding why certain cases failed
Spotting patterns in the failures

This is where you learn what's actually wrong with your prompt.
Step 5: Iterate and Improve

Based on your analysis, you update the prompt to address the failures. Then you run the eval again to see if the changes helped. This creates a feedback loop of continuous improvement.
The Grading Problem

One of the trickiest parts of evaluation is the grading step. How do you determine if an output is "good"?

There are three main approaches:


Code grading - Programmatic checks that can verify exact criteria

> *(See full lesson at course URL)*

#### 16. Generating test datasets

Generating high-quality test datasets is often the most challenging part of prompt evaluation. The old saying "garbage in, garbage out" applies directly - if your test data doesn't represent real inputs, your evaluation results won't be meaningful.

Why Test Data Matters

Your evaluation is only as good as your test data. If you evaluate against inputs that don't match what real users will send, you'll optimize for the wrong behavior. This can lead to nasty surprises when your prompt goes live.


Good test data should:


Cover the distribution of real inputs - Include common cases, edge cases, and everything in between
Represent actual user behavior - Think about how users actually phrase their requests
Include hard cases - Don't just test easy inputs that you know will work

Generating Test Data with Claude

One effective approach is to use Claude itself to generate your test dataset. This works especially well when you're just starting to build an application and don't yet have real user data.

Here's the pattern:

You provide Claude with a description of your application and the kinds of tasks it should handle. Claude then generates a diverse set of test cases that cover the expected input space.

For example, if you're building a code generation prompt, you might give Claude a description like:


"Generate 20 test cases for a prompt that converts natural language to Python regular expressions. Include cases covering different complexity levels, various types of patterns, and edge cases like empty input."

Claude can generate comprehensive test datasets quickly. The key is being specific about what you want covered.
Structuring Test Cases

Each test case in your dataset should be a structured object containing:


task - The input or instruction to test against
metadata - Additional context like category, difficulty, or expected behavior

For batch evaluation, you'll also want to include:

expected - Optional correct output for comparison
test_type - Category label for grouping results

Building a Dataset File

After generating test cases, save them to a JSON file that your eval pipeline can load:


with open("dataset.json", "w") as f:
    json.dump(dataset, f, indent=2)

This gives you a reusable, version-controlled dataset that you can run through your eval pipeline multiple times as you iterate on your prompts.
Iterating on Test Data

Your test dataset should evolve over time:


Add cases for bugs you catch in production

> *(See full lesson at course URL)*

#### 17. Running the eval

Now that we have our evaluation dataset ready, it's time to build the core evaluation pipeline. This involves taking each test case, merging it with our prompt, feeding it to Claude, and then grading the results.


The evaluation process follows a clear workflow: we take our dataset of test cases, combine each one with our prompt template, send it to Claude for processing, and then evaluate the output using a grader system.
Building the Core Functions


The evaluation pipeline consists of three main functions, each with a specific responsibility. Let's start with the simplest one - the function that handles individual prompts.

The run_prompt Function

This function takes a test case and merges it with our prompt template:

def run_prompt(test_case):
    """Merges the prompt and test case input, then returns the result"""
    prompt = f"""Please solve the following task: {test_case["task"]}"""
    messages = []
    add_user_message(messages, prompt)
    output = chat(messages)
    return output

Right now, we're keeping the prompt extremely simple. We're not including any formatting instructions, so Claude will likely return more verbose output than we need. We'll refine this later as we iterate on our prompt design.
The run_test_case Function


This function orchestrates running a single test case and grading the result:


def run_test_case(test_case):
    """Calls run_prompt, then grades the result"""
    output = run_prompt(test_case)
    # TODO - Grading
    score = 10
    return {
        "output": output,
        "test_case": test_case,
        "score": score
    }

For now, we're using a hardcoded score of 10. The grading logic is where we'll spend significant time in upcoming sections, but this placeholder lets us test the overall pipeline.
The run_eval Function

This function coordinates the entire evaluation process:

def run_eval(dataset):
    """Loads the dataset and calls run_test_case with each case"""
    results = []
    for test_case in dataset:
        result = run_test_case(test_case)
        results.append(result)
    return results

This function processes every test case in our dataset and collects all the results into a single list.
Running the Evaluation


To execute our evaluation pipeline, we load our dataset and run it through our functions:


with open("dataset.json", "r") as f:
    dataset = json.load(f)
results = run_eval(dataset)



> *(See full lesson at course URL)*

#### 18. Model based grading

When building prompt evaluation workflows, grading systems provide objective signals about output quality. A grader takes model output and returns some kind of measurable feedback - typically a number between 1 and 10, where 10 represents high quality and 1 represents poor quality.

Types of Graders

There are three main approaches to grading model outputs:


Code graders - Programmatically evaluate outputs using custom code
Model graders - Use another AI model to assess the quality
Human graders - Have people manually review and score outputs

Code Graders

Code graders let you implement any programmatic check you can imagine. Common uses include:

Checking output length
Verifying output does or doesn't contain certain words
Syntax validation for JSON, Python, or regex
Readability scores to ensure appropriate reading levels

Model Graders

Model graders offer tremendous flexibility by using an additional API call to evaluate outputs. They're useful for assessing:


Response quality
Quality of instruction following
Completeness
Helpfulness
Safety

Human Graders

Human graders provide the most flexibility but come with significant downsides. While humans can evaluate responses for any criteria imaginable, the process is time-consuming and tedious.
Defining Evaluation Criteria

Before implementing any grader, you need clear evaluation criteria. For a code generation prompt, you might focus on:


Format - Should return only Python, JSON, or Regex without explanation
Valid Syntax - Produced code should have valid syntax
Task Following - Response should directly address the user's task with accurate code

The first two criteria work well with code graders, while task following is better suited for model graders due to their flexibility.
Implementing a Model Grader


Model graders are often the easiest to implement. Here's a basic structure:


def grade_by_model(test_case, output):
    messages = []
    add_user_message(messages, eval_prompt)
    add_assistant_message(messages, "```json")
    eval_text = chat(messages, stop_sequences=["```"])
    return json.loads(eval_text)

The grading prompt should be comprehensive and include:


Clear role definition for the grader
The original task
The AI-generated solution to evaluate
Specific output format requirements


> *(See full lesson at course URL)*

#### 19. Code based grading

When evaluating AI models that generate code, you need more than just checking if the response makes sense. You also need to verify that the generated code actually has valid syntax and follows the correct format. This is where code-based grading comes in.

How Code Grading Works

Code grading validates two key aspects of AI-generated responses:


Format - The response should return only the requested code type (Python, JSON, or Regex) without explanations
Valid Syntax - The generated code should actually parse correctly as the intended language
Task Following - The response should directly address what was asked and be accurate

The first two criteria are handled by the code grader, while task following is evaluated by the model grader. Together, they provide a comprehensive evaluation.


Syntax Validation Functions


To check if generated code has valid syntax, you can create three helper functions that attempt to parse the output:


def validate_json(text):
    try:
        json.loads(text.strip())
        return 10
    except json.JSONDecodeError:
        return 0


def validate_python(text):
    try:
        ast.parse(text.strip())
        return 10
    except SyntaxError:
        return 0


def validate_regex(text):
    try:
        re.compile(text.strip())
        return 10
    except re.error:
        return 0

Each function tries to parse the text as its respective format. If parsing succeeds, it returns a perfect score of 10. If it fails with an error, the syntax is invalid and returns 0.
Dataset Format Requirements

For the code grader to know which validator to use, your test cases need to specify the expected output format:

{"task": "Create a Python function to validate an AWS IAM username", "format": "python"}

You can update your dataset generation prompt to automatically include this format field by adding it to the example output structure.
Improving Prompt Clarity

To get better results from your AI model, make your prompt instructions more specific about the expected output format:


* Respond only with Python, JSON, or a plain Regex
* Do not add any comments or commentary or explanation


You can also use a pre-filled assistant message with code blocks to encourage the model to return just the raw code:


add_assistant_message(messages, "```code")

This tells Claude to start generating code content without having to specify whether it's Python, JSON, or Regex upfront.
Combining Scores


> *(See full lesson at course URL)*


### 📖 Prompt engineering techniques

#### 21. Being clear and direct

The first line of your prompt is the most important part of your entire request. This is where you set the stage for everything that follows, and getting it right can dramatically improve your results.

Being Clear and Direct

"When crafting that crucial first line, you want to focus on two key principles: clarity and directness. This means using simple language that leaves no room for ambiguity about what you want Claude to do."
Clear Communication

"Being \"clear\" means:"
- Use simple language that anyone can understand
- State exactly what you want without beating around the bush
- Lead with a straightforward statement of Claude's task

"Instead of writing something vague like \"I need to know about those things people put on their roofs that use sun - those solar panel things, I think they're called,\" be direct and write: \"Write three paragraphs about how solar panels work.\""
Direct Instructions

"Being \"direct\" focuses on how you structure your request:"
- Use instructions, not questions
- Start with direct action verbs like "Write," "Create," or "Generate"

"Rather than asking \"I was reading about renewable energy and geothermal energy sounds neat. What countries use it?\" try: \"Identify three countries that use geothermal energy. Include generation stats for each.\""
Putting It Into Practice

Let's see this technique in action. Starting with a weak prompt that simply asked "What should this person eat?" we can apply our clear and direct approach.
"The improved version becomes:"
Generate a one-day meal plan for an athlete that meets their dietary restrictions.

"This revision immediately tells Claude:"
- What action to take (generate)
- What to create (a meal plan)
- Key constraints (one day, for an athlete, meeting dietary restrictions)
Results Matter

This simple change can make a significant difference in output quality. In our example, the evaluation score jumped from 2.32 to 3.92 - a substantial improvement from just restructuring that opening line.
"The key takeaway: start strong with a clear, direct statement that uses an action verb and explicitly defines the task. This sets Claude up for success and gives you much better results right from the start.

#### 22. Introducing tool use

Tools allow Claude to access information from the outside world, extending its capabilities beyond what it learned during training. By default, Claude only knows information from its training data and can't access current events, real-time data, or external systems. Tool use solves this limitation by creating a structured way for Claude to request and receive fresh information.

The Problem Without Tools

When users ask Claude for current information, it hits a wall. For example, if someone asks "What's the weather in San Francisco, California?" Claude has to respond with something like "I'm sorry, but I don't have access to up-to-date weather information."

This creates a frustrating user experience when people need real-time data that Claude could theoretically help with if it just had access to current information.

How Tool Use Works

"Tool use follows a specific back-and-forth pattern between your application and Claude. Here's the complete flow:"

"Initial Request:" You send Claude a question along with instructions on how to get extra data from external sources

"Tool Request:" Claude analyzes the question and decides it needs additional information, then asks for specific details about what data it needs

"Data Retrieval:" Your server runs code to fetch the requested information from external APIs or databases

"Final Response:" You send the retrieved data back to Claude, which then generates a complete response using both the original question and the fresh data

Weather Example in Practice

"Let's see how this works with the weather question. The process becomes much more specific:"

When a user asks about current weather, you include details in your prompt about how to retrieve weather data. Claude recognizes it needs current information and requests weather data for the specific location. Your server then calls a weather API to get real-time conditions and sends that data back to Claude. Finally, Claude combines the fresh weather data with the user's question to provide an accurate, current response.

Key Benefits

"Real-time Information:" Access current data that wasn't available during Claude's training

"External System Integration:" Connect Claude to databases, APIs, and other services

"Dynamic Responses:" Provide answers based on the most up-to-date information available

"Structured Interaction:" Claude knows exactly what information it needs and how to ask for it


> *(See full lesson at course URL)*

#### 23. Project overview

We're going to build a practical project that teaches Claude how to set reminders for future dates. This might sound simple at first, but it reveals several interesting challenges that we'll solve using custom tools.

"The goal is to have a conversation like this: you tell Claude "Set a reminder for my doctor's appointment. It's a week from Thursday," and Claude responds "OK, I will remind you." To make this work, we need to understand why this is actually harder than it looks."

Why This Is Challenging

"Claude has some built-in knowledge about dates and times, but it also has some significant limitations:"

Claude might know the current date, but not the exact time

Claude doesn't always handle time-based addition well, especially if looking many days into the future

Claude doesn't know how to set a reminder!

These limitations mean that even a simple request like "set a reminder for 24 hours from now" becomes problematic. Claude doesn't know what "24 hours from now" actually means without knowing the current time. And even if it could calculate the right date, it has no mechanism to actually create a reminder.

Tools We Need

"To solve these problems, we'll create three custom tools that work together:"

Get the Current Date Time

This is our starting tool - it gives Claude access to both the current date and the exact time. This solves the problem of Claude not knowing when "now" actually is.

Add Duration to Date Time

This tool handles the math of adding time periods to dates. Instead of relying on Claude to correctly calculate "what date is 379 days from January 13th, 1973," we give it a reliable tool that can handle these calculations accurately.

Set a Reminder

Finally, we need a way for Claude to actually create reminders. This tool will provide the mechanism that Claude lacks for setting up future notifications.

We'll implement these tools one at a time, starting with the datetime tool to understand how tool calling works, then building up to the more complex functionality. By the end, Claude will be able to handle natural language requests about setting reminders and convert them into actual scheduled notifications.

#### 24. Tool functions

When building AI applications with Claude, you'll often need to give it access to real-time information or the ability to perform actions. This is where tool functions come in - they're Python functions that Claude can call when it needs additional data to help users.

"The image above shows three essential tools we'll be implementing: getting the current date/time, adding duration to dates, and setting reminders. Let's start with the first one."

What Are Tool Functions?

A tool function is a plain Python function that gets executed automatically when Claude determines it needs extra information to complete a task. For example, if a user asks "What time is it?", Claude would call your date/time tool to get the current time.

Here's an example of a weather tool function. Notice how it validates inputs and provides clear error messages - these are key best practices we'll follow.

Best Practices for Tool Functions

"When writing tool functions, keep these guidelines in mind:"

"Use descriptive names:" Both your function name and parameter names should clearly indicate their purpose

"Validate inputs:" Always check that required parameters are present and valid

"Provide meaningful error messages:" If Claude gets an error, it might try calling your function again with corrected parameters

The error handling is particularly important because Claude can learn from failures. If you return a clear error message like "Location cannot be empty", Claude might retry the function call with a proper location value.

Building Your First Tool Function

"Let's create a function to get the current date and time. This function will accept a format string to control how the date appears:"

The validation check

ensures we don't try to format a date with an empty string. While Claude rarely makes this mistake, providing clear error messages helps the AI understand what went wrong and how to fix it.

When Claude encounters an error, it sees the exact error message. This feedback loop allows Claude to adjust its approach and try again with corrected parameters.

Next Steps

This tool function is just the first step. Next, you'll need to create a JSON schema that describes this function to Claude, then integrate it into your chat system. The function itself is straightforward Python - the complexity comes in properly connecting it to Claude's tool-calling system.

#### 25. Tool schemas

After writing your tool function, the next step is creating a JSON schema that tells Claude what arguments your function expects and how to use it. This schema acts as documentation that Claude reads to understand when and how to call your tools.

Understanding JSON Schema

JSON Schema isn't specific to AI or tool calling - it's a widely-used data validation specification that's been around for years. The AI community adopted it because it's a convenient way to describe function parameters and validate data.

"The complete tool specification has three main parts:"

"name" - The function name (like "get_weather")

"description" - What the tool does and when to use it

"input_schema" - The actual JSON schema describing the arguments

Writing Effective Descriptions

"The description field is crucial for helping Claude understand your tool. Follow these best practices:"

Explain what the tool does, when to use it, and what it returns

Aim for 3-4 sentences

Provide detailed descriptions for each argument as well

The input_schema section describes your function's parameters using standard JSON Schema format, including type information and detailed descriptions for each argument.

The Easy Way: Let Claude Write Your Schema

"Instead of writing JSON schemas from scratch, you can use Claude itself to generate them. Here's the process:"

Copy your tool function

Go to Claude and ask it to write a JSON schema for tool calling

Include the Anthropic documentation on tool use as context

Let Claude generate a properly formatted schema following best practices

"The prompt should be something like: "Write a valid JSON schema spec for the purposes of tool calling for this function. Follow the best practices listed in the attached documentation.""

Implementing the Schema in Code

Once Claude generates your schema, copy it into your code file. Use a consistent naming pattern like

to keep things organized:

Adding Type Safety

For better type checking, import and use the

type from the Anthropic library:

This isn't strictly necessary for functionality, but it prevents type errors when you use the schema later in your code.

The combination of a well-written tool function and a detailed JSON schema gives Claude everything it needs to understand and properly use your tools in conversations.

#### 26. Handling message blocks

When working with Claude's tool functionality, you'll encounter a new type of response structure that's different from the simple text responses you've seen before. Instead of just getting back a single text block, Claude can now return multi-block messages that contain both text and tool usage information.

Making Tool-Enabled API Calls

To enable Claude to use tools, you need to include a

parameter in your API call. Here's how to structure the request:

The

parameter takes a list of JSON schemas that describe the available functions Claude can call.

Understanding Multi-Block Messages

When Claude decides to use a tool, it returns an assistant message with multiple blocks in the content list. This is a significant change from the simple text-only responses you've worked with before.

"A multi-block message typically contains:"

"Text Block" - Human-readable text explaining what Claude is doing (like "I can help you find out the current time. Let me find that information for you")

"ToolUse Block" - Instructions for your code about which tool to call and what parameters to use

"The ToolUse block includes:"

An ID for tracking the tool call

The name of the function to call (like "get_current_datetime")

Input parameters formatted according to your JSON schema

The type designation "tool_use"

Handling Message History with Multi-Block Content

"Here's the critical part: Claude doesn't store conversation history, so you must manage it manually. When working with tool responses, you need to preserve the entire content structure, including all blocks."

"Instead of just extracting text, you need to append the complete response content:"

This preserves both the text block and the tool use block, maintaining the full conversation context for future API calls.

The Complete Flow

"The tool usage process follows this pattern:"

Send user message with tool schema to Claude

Receive multi-block assistant message (text + tool use)

Extract tool call information and execute the function

Send tool result back to Claude with complete message history

Receive final response from Claude

Each step requires careful handling of the message structure to maintain conversation continuity. The key insight is that tool-enabled conversations involve more complex message formats, but the fundamental principle of maintaining complete message history remains the same.

Updating Helper Functions

If you've been using helper functions like

and


> *(See full lesson at course URL)*

#### 27. Sending tool results

After Claude requests a tool call, you need to execute the function and send the results back. This completes the tool use workflow by providing Claude with the information it requested.

Running the Tool Function

When Claude responds with a tool use block, you extract the input parameters and call your function. Here's how to access the tool parameters:

The double asterisk (

) unpacks the dictionary into keyword arguments that your function expects.

Tool Result Block

After running the tool, you send the results back to Claude using a tool result block. This block has several important properties:

tool_use_id - Must match the ID from the original tool use block

content - The output from your tool function, converted to a string

is_error - Set to true if an error occurred during execution

Handling Multiple Tool Calls

Claude can request multiple tool calls in a single response. For example, if a user asks "What's 10 + 10 and what's 30 + 30?", Claude might send two separate tool use blocks:

Each tool use block gets a unique ID, and you must match these IDs when sending back results:

This ID system ensures Claude can correctly match each result with its corresponding request, even if the results arrive in a different order.

Sending the Follow-up Request

Your follow-up request to Claude must include the complete conversation history plus the new tool result:

The conversation flow looks like this:

Remember to include the tool schema in your follow-up request, even though Claude probably won't need to use tools again. Claude needs the schema to understand the tool references in the conversation history.

Complete Workflow

Here's the full process:

User asks a question requiring tool use

Claude responds with a tool use block

You execute the requested tool function

You send a follow-up request with the tool result

Claude provides a final answer using the tool output

The final request includes your complete message history, the tool result block, and the tool schema. Claude then responds with a regular text message that incorporates the information from your tool execution.

#### 28. Multi-turn conversations with tools

When building applications with multiple tools, you need to handle scenarios where Claude might need to call several tools in sequence to answer a single user question. For example, if a user asks "What day is 103 days from today?", Claude needs to first get the current date, then add 103 days to it.

This creates a multi-turn conversation pattern where Claude makes multiple tool requests before providing a final answer. Your application needs to handle this automatically.

The Multi-Turn Tool Pattern

Here's what happens behind the scenes when Claude needs multiple tools:

User asks: "What day is 103 days from today?"

Claude responds with a tool use block requesting

got_current_datetime

Your server calls the function and returns the result

Claude realizes it needs more information and requests

add_duration_to_datetime

Your server calls that function and returns the result

Claude now has enough information to provide the final answer

Building a Conversation Loop

To handle this pattern, you need a conversation loop that continues until Claude stops requesting tools:

Refactoring Helper Functions

Before implementing the conversation loop, you need to update your helper functions to handle multiple message blocks properly.

Updating Message Handlers

Your

and

functions currently assume they're always working with plain text. Update them to handle full message objects:

This allows you to pass in either a string, a list of blocks, or a complete message object.

Updating the Chat Function

Modify your chat function to accept a list of tools and return the full message instead of just text:

Extracting Text from Messages

Since the chat function now returns full messages instead of just text, add a helper to extract text when needed:

This function finds all text blocks in a message and joins them together, which is useful when you need to display the final response to users.

Why These Changes Matter

These refactoring steps prepare your code for the reality of tool-enabled conversations:

Multiple blocks per message - Claude's responses can contain both text and tool use blocks

Flexible message handling - Your functions can now work with various message formats

Full message preservation - You maintain all the information Claude provides, not just the text portions

Tool list support - Your chat function can now receive and use multiple tools


> *(See full lesson at course URL)*

#### 29. Implementing multiple turns

Building a conversation system with tools requires implementing a loop that keeps calling Claude until it stops requesting tool usage. When Claude no longer asks for tools, that signals it has a final response ready for the user.

Detecting Tool Requests

The key to knowing whether Claude wants to use a tool lies in the

field of the response message. When Claude decides it needs to call a tool, this field gets set to

. This gives us a clean way to check if we need to continue the conversation loop:

The Conversation Loop

The main conversation function follows a simple pattern:

This loop continues until Claude provides a final answer without requesting any tools.

Handling Multiple Tool Calls

Claude can request multiple tools in a single response. The message content contains a list of blocks, and we need to process each tool use block separately:

The

function handles this by filtering for tool use blocks and processing each one:

Tool Result Blocks

For each tool use block, we need to create a corresponding tool result block. These blocks have specific required fields:

The tool result block must include the same ID as the original tool use block, but in the

field:

Error Handling

Robust tool execution requires handling potential errors. When a tool fails, we still need to return a tool result block, but with error information:

Scalable Tool Routing

To support multiple tools, create a separate routing function instead of hardcoding tool names:

This approach makes it easy to add new tools without modifying the core conversation logic.

Complete Workflow

The complete multi-turn conversation works like this:

Send user message to Claude with available tools

Claude responds with text and/or tool use blocks

Execute any requested tools and create tool result blocks

Send tool results back to Claude as a user message

Repeat until Claude provides a final response without tool requests

This creates a seamless experience where Claude can make multiple tool calls across several conversation turns to gather all the information needed before providing a comprehensive final answer to the user.

#### 30. Using multiple tools

Adding multiple tools to your Claude implementation becomes straightforward once you have the core tool-handling infrastructure in place. This tutorial shows how to integrate additional tools by following a simple pattern.

The Tools We're Adding

We need three main capabilities for our reminder system:

Get current date time - Claude needs to know the current date and time

Add duration to date time - Claude isn't perfect with date time addition

Set a reminder - Need a way to set a reminder

The good news is that most of the implementation work is already done. The

function handles various time units (seconds, minutes, hours, days, weeks, months) and returns properly formatted datetime strings.

The

function is a simple placeholder that prints out confirmation details rather than actually setting system reminders.

Adding Tools to the Conversation

The process follows the same pattern we established earlier. First, update the

function to include the new tool schemas:

This tells Claude about all available tools it can use during the conversation.

Handling Tool Execution

Next, update the

function to handle the new tool calls:

The pattern is consistent: check the tool name, call the corresponding function with the provided input, and return the result.

Testing Multiple Tool Usage

Let's test with a complex request that requires multiple tools: "Set a reminder for my doctors appointment. Its 177 days after Jan 1st, 2050."

This request forces Claude to:

Calculate the date 177 days after January 1st, 2050

Set a reminder for that calculated date

Claude handles this by first explaining what it needs to do, then using the

tool to calculate June 27, 2050, and finally calling

with the correct date.

Understanding the Message Flow

Looking at the conversation history reveals how Claude manages multiple tools in a single response. The assistant message contains both a text block explaining the process and a tool use block for the first calculation.

After receiving the tool result, Claude continues with another message containing both text and another tool use block for setting the reminder. This demonstrates how Claude can chain multiple tool calls together to complete complex tasks.

Key Takeaways

Once you have the basic tool infrastructure set up, adding new tools follows a simple three-step process:

Add the tool schema to the tools list in

Add a case for the new tool in the

function

Implement the actual tool function


> *(See full lesson at course URL)*

#### 31. The batch tool

When working with Claude's tool calling capabilities, you might notice that Claude can include multiple tool use blocks in a single assistant message. This allows Claude to run several tools in parallel rather than making separate requests for each one. However, getting Claude to actually do this consistently can be challenging in practice.

The Problem with Multiple Tool Calls

Let's say you ask Claude to set two reminders for the same date. Theoretically, Claude should be able to send back a single response containing two tool use blocks - one for each reminder. But in reality, Claude often sends separate responses instead.

What typically happens is Claude makes the first tool call, waits for the result, then makes the second tool call in a follow-up message. This creates unnecessary back-and-forth communication when the operations could have been done simultaneously.

The Batch Tool Solution

The solution is to implement a "batch tool" - a special tool that accepts a list of other tool calls to execute simultaneously. This is essentially a workaround that tricks Claude into making multiple tool calls at once.

Here's how it works:

You define a batch tool schema that tells Claude it can run multiple other tools in parallel

Instead of calling tools directly, Claude calls the batch tool with a list of tool invocations

Your code processes this list and executes each tool call

You return the combined results back to Claude

Implementing the Batch Tool Schema

The batch tool schema defines how Claude should structure its requests when it wants to run multiple tools:

Processing Batch Tool Calls

When Claude uses the batch tool, you need to process the list of invocations and execute each one. Here's the implementation:

You'll also need to update your main tool routing function to handle batch tool calls:

Results

With the batch tool implemented, Claude is much more likely to group related operations together. Instead of making separate requests for each reminder, Claude will use the batch tool to set both reminders simultaneously.

The conversation flow becomes much cleaner - one request from the user, one response from Claude with the batch tool call, and one follow-up with all the results. This reduces latency and makes your application more efficient.

While it might seem like a workaround (and it is), the batch tool pattern is an effective way to encourage Claude to think about operations that can be parallelized and execute them more efficiently.

#### 32. Tools for structured data

When you need structured data from Claude, you have two main approaches: prompt-based techniques using message prefills and stop sequences, or a more robust method using tools. While the prompt-based approach is simpler to set up, tools provide more reliable output at the cost of additional complexity.

Tools for Structured Data

The tool-based approach works by creating a JSON schema that defines the exact structure of data you want to extract. Instead of hoping Claude formats its response correctly, you're essentially giving Claude a function to call with specific parameters that match your desired output structure.

Here's how the process works:

Write a schema that describes the structure of data you're looking for

Force Claude to use a tool with the

parameter

Extract the structured data from the tool use response

No need to provide a follow-up response - you're done once you get the data

For example, if you want to extract a financial balance and key insights from a statement, your schema would define those as an integer and array of strings respectively.

Controlling Tool Use

A critical part of this technique is ensuring Claude actually calls your tool. You can control this behavior using the

parameter:

- Model decides if it needs to use a tool (default)

- Model must use a tool, but can choose which one

- Model must use the specified tool

For structured data extraction, you'll typically want the third option to guarantee Claude calls your specific schema tool.

Implementation Example

Let's say you want to extract a title, author, and key insights from an article. First, you'd create a tool schema:

Then you'd call Claude with the tool and force its use:

The response will contain a tool use block with your structured data in the

field. You can access it directly:

When to Use Each Approach

Choose prompt-based structured output when you need something quick and simple. Use tools when you need guaranteed reliability and can handle the extra setup complexity. Both techniques are valuable depending on your specific use case and requirements.

#### 33. The text edit tool

Important Note: Tool version strings can for all model versions can be found here: https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/text-editor-tool

Claude comes with one built-in tool that you don't need to create from scratch: the text editor tool. This tool gives Claude the ability to work with files and directories just like you would in a standard text editor.

What the Text Editor Tool Can Do

The text editor tool provides Claude with a comprehensive set of file manipulation capabilities:

View file or directory contents

View specific ranges of lines in a file

Replace text in a file

Create new files

Insert text at specific lines in a file

Undo recent edits to files

This dramatically expands Claude's abilities and essentially gives it the power to act as a software engineer right out of the gate.

Understanding the Implementation Requirements

Here's where things get a bit confusing: while the tool schema is built into Claude, you still need to provide the actual implementation. Think of it this way - Claude knows how to ask for file operations, but you need to write the code that actually performs those operations.

When you use custom tools, you typically write both the JSON schema and the function implementation. With the text editor tool, Claude provides the schema knowledge, but you must write functions to handle Claude's requests to create files, read directories, replace text, and so on.

Schema Versions

You do need to include a small schema stub when using the text editor tool, and the exact schema depends on which Claude model you're using:

Claude automatically expands this small schema into a much larger, detailed specification that includes all the parameters and operations available.

Practical Example

Let's see the text editor tool in action. When you ask Claude to work with files, it will use the tool to read, modify, and create files as needed.

For example, if you ask Claude to "Open the ./main.py file and summarize its contents", Claude will:

Use the text editor tool to view the file

Read the contents

Provide you with a summary

You can take this further and ask Claude to modify files. For instance: "Open the ./main.py file and write out a function to calculate pi to the 5th digit. Then create a ./test.py file to test your implementation."

Claude will:

View the existing main.py file

Replace its contents with a new implementation including the pi calculation function


> *(See full lesson at course URL)*

#### 34. The web search tool

Claude includes a built-in web search tool that lets it search the internet for current or specialized information to answer user questions. Unlike other tools where you need to provide the implementation, Claude handles the entire search process automatically - you just need to provide a simple schema to enable it.

Setting Up the Web Search Tool

To use the web search tool, you create a schema object with these required fields:

The

field limits how many searches Claude can perform. Claude might do follow-up searches based on initial results, so this prevents excessive API calls.

How It Works

When you include the web search schema in your tools list, Claude will automatically decide when to search based on your question. For example, asking "What's the best exercise for gaining leg muscle?" might trigger a search for current fitness research.

The response contains several types of blocks:

- TextBlock - Claude's explanation of what it's doing

- ServerToolUseBlock - Shows the exact search query Claude used

- WebSearchToolResultBlock - Contains the search results

- WebSearchResultBlock - Individual search results with titles and URLs

- CitationsWebSearchResultLocation - Specific text citations supporting Claude's statements

Restricting Search Domains

You can limit searches to specific domains using the

field. This is particularly useful when you want authoritative sources:

This ensures Claude only searches trusted domains like government health sites instead of random fitness blogs with potentially unreliable information.

Rendering Search Results

The response structure is designed for rich UI rendering. You typically:

Display text blocks as regular content

Show web search results as a reference list at the top

Render citations inline with links back to source material

Highlight cited text to show how Claude supports its statements

This creates a transparent experience where users can verify Claude's sources and understand how it arrived at its conclusions. The citation system helps build trust by showing the evidence behind Claude's responses.

#### 35. Introducing Retrieval Augmented Generation

Retrieval Augmented Generation (RAG) is a technique that helps you work with large documents when using Claude. Instead of cramming an entire 800-page financial report into a single prompt, RAG lets you intelligently find and include only the most relevant sections for each question.

The Problem with Large Documents

Imagine you have a massive financial document and want to ask Claude specific questions about it, like "What risk factors does this company have?" You face a fundamental challenge: how do you get the right information from the document into Claude so it can answer your question effectively?

Option 1: Include Everything in the Prompt

The first approach seems straightforward - extract all the text from the document and stuff it directly into your prompt along with the user's question.

This approach has several problems:

There's a hard limit on how much text Claude can process - your document might be too long

Claude becomes less effective with very long prompts

Larger prompts cost more money and take longer to process

Option 2: Break Documents into Chunks

The second approach is more sophisticated. You break the document into smaller chunks during a preprocessing step, then find and include only the chunks relevant to each user question.

Here's how it works: when a user asks "What risks does this company face?", you search through your chunks to find the one about "Risk Factors" and include only that section in your prompt to Claude.

Benefits of the Chunking Approach

Claude can focus on only the most relevant content

Scales up to very large documents

Works with multiple documents

Smaller prompts cost less and run faster

Challenges with Chunking

Requires a preprocessing step to split documents

Need a searching mechanism to find "relevant" chunks

Included chunks might not contain all the context Claude needs

Many ways to chunk text - which approach is best?

For example, if you only include the "Risk Factors" section, you might miss important context from the "Strategy Outlook" section that addresses how the company plans to handle those risks.

This is RAG

Option 2 is Retrieval Augmented Generation. Despite its complexity, RAG offers significant advantages for working with large documents, but it comes with technical challenges that require careful consideration.

The key components of RAG are:

Document preprocessing and chunking

A search mechanism to find relevant chunks


> *(See full lesson at course URL)*

#### 36. Text chunking strategies

Text chunking is one of the most critical steps in building a RAG (Retrieval Augmented Generation) pipeline. How you break up your documents directly impacts the quality of your entire system. A poor chunking strategy can lead to irrelevant context being inserted into your prompts, causing your AI to give completely wrong answers.

Consider this example: you have a document with sections on medical research and software engineering. If you chunk poorly, a user asking "How many bugs did engineers fix this year?" might get information about medical research instead of software engineering, simply because the medical section happened to contain the word "bug" in a different context.

This demonstrates why chunking strategy matters so much. The goal is to create chunks that maintain semantic coherence and provide meaningful context when retrieved.

Three Main Chunking Strategies

There are three primary approaches to chunking text, each with distinct advantages and trade-offs:

Size-based: Divide text into strings of equal length

Structure-based: Split based on document structure (headers, paragraphs, sections)

Semantic-based: Group related sentences or sections using NLP techniques

Size-Based Chunking

Size-based chunking is the most straightforward approach. You simply divide your document into chunks of approximately equal character or word count. It's easy to implement and works reliably across different document types.

However, this approach has clear downsides. Words get cut off mid-sentence, and chunks lose important context. For example, a chunk might not include the section header that would explain what the content is actually about.

The solution is to add overlap between chunks. Each chunk includes some characters from neighboring chunks, ensuring better context preservation and avoiding abrupt cutoffs.

Here's a basic implementation of character-based chunking with overlap:

Structure-Based Chunking

Structure-based chunking leverages the natural organization of your documents. If you're working with markdown files, you can split on headers. For other formats, you might split on paragraphs or other structural elements.

This approach works beautifully when you have guarantees about document structure. For markdown documents, you can split on section headers:

The major limitation is that many documents don't have consistent structure. Plain text files, PDFs, or user-uploaded documents might not have clear structural markers to split on.


> *(See full lesson at course URL)*

#### 37. Text embeddings

After extracting text chunks from a document, the next step in a RAG pipeline is finding which chunks are most relevant to a user's question. This is essentially a search problem - you need to look through all your chunks and identify the ones that relate to what the user is asking about.

Semantic Search

The most common approach for finding relevant chunks is semantic search. Unlike traditional keyword-based search, semantic search uses text embeddings to understand the actual meaning of both the user's question and each text chunk. This allows the system to find conceptually related content even when the exact words don't match.

What Are Text Embeddings?

A text embedding is a numerical representation of the meaning contained in some text. Think of it as converting words and sentences into a format that computers can work with mathematically.

Here's how the process works:

You feed text into an embedding model

The model outputs a long list of numbers (the embedding)

Each number ranges from -1 to +1

These numbers represent different qualities or features of the input text

Understanding the Numbers

Each number in an embedding is essentially a "score" for some quality of the input text. However, here's the important caveat: we don't actually know what each specific number represents.

While it's helpful to imagine that one number might represent "how happy the text is" and another might represent "how much the text talks about oceans," these are just conceptual examples. The embedding model learns these features during training, but they're not explicitly labeled or interpretable to us.

Despite this opacity, embeddings are incredibly powerful because they capture semantic meaning in a way that allows for mathematical comparison between different pieces of text.

Embeddings on Vertex AI

Claude can't generate embeddings directly. Instead, you need to use a specialized embedding model. On Vertex AI, the model we'll use is called text-embedding-005.

Implementation

To work with embeddings on Vertex AI, you'll need to install the Google GenAI SDK:

When you run this function with a text chunk, you'll get back a list of floating-point numbers representing the semantic meaning of that text. These embeddings form the foundation for implementing semantic search in your RAG system.


> *(See full lesson at course URL)*

#### 38. The full RAG flow

Now that we've covered the basics of RAG, text chunking, and embeddings, let's walk through the complete RAG pipeline step by step. This detailed example will show you exactly how all the pieces fit together in a real implementation.

Step 1: Chunk Your Source Text

First, we take our source document and break it into manageable chunks. For this example, we'll use two simple text sections:

Section 1: Medical Research - "This year saw significant strides in our understanding of XDR-47, a 'bug' we have not seen before."

Section 2: Software Engineering - "This division dedicated significant effort to studying various infection vectors in our distributed systems"

Step 2: Generate Embeddings

Next, we convert each text chunk into numerical embeddings. To make this easier to understand, let's imagine we have a perfect embedding model that always returns exactly two numbers, and we know what each number represents:

In our imaginary model:

First number: How much the text talks about medicine

Second number: How much the text talks about software engineering

So our medical research section gets [0.97, 0.34] - very medical, somewhat software-related due to the word "bug". The software engineering section gets [0.30, 0.97] - very software-focused, but "infection vectors" has medical connotations.

Normalization

Before storing these embeddings, they go through a normalization process that scales each vector to have a magnitude of 1.0. This is typically handled automatically by your embedding API, but it's important to understand it happens.

After normalization, our embeddings become [0.944, 0.331] and [0.295, 0.955]. We can visualize these on a unit circle where both points lie exactly on the circle's edge.

Step 3: Store in Vector Database

The normalized embeddings get stored in a vector database - a specialized database optimized for storing, comparing, and searching through long lists of numbers like our embeddings.

At this point, we pause. All the work so far has been preprocessing that happens ahead of time. Now we wait for a user to submit a query.

Step 4: Process User Query

When a user asks a question like "I'm curious about the company. In particular, what did the software engineering dept do this year?", we run their query through the same embedding model.

This query gets embedded as [0.1, 0.89] - low medical score, high software engineering score. After normalization, it becomes [0.112, 0.993].

Step 5: Find Similar Embeddings


> *(See full lesson at course URL)*

#### 39. Implementing the RAG flow

Now that we understand the RAG flow conceptually, let's implement it step by step using a practical example. We'll work through all five stages of the RAG process, from chunking text to finding relevant documents for user queries.

Setting Up the Vector Database

For this implementation, we'll use a custom VectorIndex class that provides the basic functionality we need for storing and searching embeddings. The class handles vector storage, distance calculations (using cosine similarity), and document retrieval.

The Five-Step RAG Implementation

Let's walk through each step of the RAG process:

Step 1: Chunk the Text by Section

First, we need to break our source document into manageable chunks. We'll use the same section-based chunking approach from earlier:

This splits our report.md file into logical sections that we can process individually.

Step 2: Generate Embeddings for Each Chunk

Next, we convert each text chunk into a numerical embedding that captures its semantic meaning:

These embeddings allow us to perform mathematical comparisons between different pieces of text.

Step 3: Store Embeddings in the Vector Database

Now we create our vector store and populate it with our embeddings and their associated text:

Notice that we store both the embedding and the original text content. This is crucial because when we retrieve similar embeddings later, we need access to the actual text, not just the numerical vectors. The embedding alone isn't useful to us as developers - we need the human-readable content it represents.

Step 4: Generate an Embedding for the User Query

When a user asks a question, we need to convert their query into the same embedding space as our stored documents:

Step 5: Search for Relevant Documents

Finally, we search our vector store to find the most relevant chunks:

This returns the two most similar chunks along with their cosine distance scores.

The diagram above illustrates how the vector database processes a user query. When we ask a question, it gets converted to an embedding vector, and the database finds the stored vectors that are "closest" to it in the high-dimensional space.

Understanding the Results

When we run this example with the query about the software engineering department, we get back two relevant sections:

Section 2: Software Engineering - Project Phoenix Stability Enhancements (distance: 0.71)

Methodology section (distance: 0.72)


> *(See full lesson at course URL)*

#### 40. BM25 lexical search

When building a RAG pipeline, you'll quickly discover that semantic search alone doesn't always return the best results. Sometimes you need exact term matches that semantic search might miss. The solution is to combine semantic search with lexical search using a technique called BM25.

The Problem with Semantic Search Alone

Let's say you're searching for a specific incident ID like "INC-2023-Q4-011" in a document. While this exact term appears multiple times in relevant sections, semantic search might return unrelated sections that are semantically similar but don't actually contain the specific term you're looking for.

This happens because semantic search focuses on meaning rather than exact matches. When you need precise term matching, you need a different approach.

Hybrid Search Strategy

The solution is to run two searches in parallel and merge the results:

Semantic Search - Uses embeddings and vector databases for meaning-based matching

Lexical Search - Uses classic text search for exact term matching

Merge Results - Combines both result sets for better coverage

How BM25 Works

BM25 (Best Match 25) is a popular algorithm for lexical search in RAG pipelines. Here's how it processes a search query:

The algorithm follows these key steps:

Tokenize the query - Break the user's question into individual terms

Count term frequency - See how often each term appears across all documents

Weight terms by rarity - Terms used less frequently get higher importance scores

Find best matches - Return chunks that contain more instances of the higher-weighted terms

The key insight is that rare terms like "INC-2023-Q4-011" are much more important for search than common words like "a" or "the".

Implementing BM25 Search

Here's how to set up a BM25 search system:

The BM25 implementation provides the same API as your semantic search system - both have add_document() and search() methods, making them easy to use together.

Better Search Results

When you run the same query through BM25 that failed with semantic search alone, you get much better results. Instead of returning irrelevant sections, BM25 prioritizes the sections that actually contain your specific search terms.

The algorithm correctly identifies that "INC-2023-Q4-011" is a rare, important term and ranks documents containing it much higher than documents with only common words from the query.

Next Steps


> *(See full lesson at course URL)*

#### 41. A Multi-index RAG pipeline

When you have both semantic search (vector embeddings) and lexical search (BM25) working independently, the next step is combining them into a unified search pipeline. This hybrid approach leverages the strengths of both methods to deliver more accurate results.

Creating a Unified Interface

Both search implementations share nearly identical APIs - they both have add_document() and search() methods that work the same way. This consistency makes it straightforward to wrap them in a single Retriever class.

The Retriever acts as a coordinator that forwards user queries to both indexes, collects their results, and merges them into a single ranked list.

Reciprocal Rank Fusion

The challenge is merging results from different search methods that use different scoring systems. Vector search returns cosine similarity scores, while BM25 returns relevance scores - you can't simply combine these numbers directly.

Instead, we use a technique called Reciprocal Rank Fusion (RRF). This method focuses on the rank position of results rather than their raw scores.

Here's how it works with an example. Say your vector search returns sections 2, 7, and 6 in that order, while BM25 returns sections 6, 2, and 7. To merge these:

First, create a table showing each text chunk and its rank from both search methods:

Section 2: Rank 1 from vector, rank 2 from BM25

Section 7: Rank 2 from vector, rank 3 from BM25

Section 6: Rank 3 from vector, rank 1 from BM25

Then apply the RRF formula to calculate a combined score for each chunk:

RRF_score(d) = Σ(1 / (k + rank_i(d)))

Where k is a constant (typically 60, but we'll use 1 for clearer results) and rank_i(d) is the rank of document d in the i-th search result.

For our example:

Section 2: 1.0/(1+1) + 1.0/(1+2) = 0.833

Section 7: 1.0/(1+2) + 1.0/(1+3) = 0.583

Section 6: 1.0/(1+3) + 1.0/(1+1) = 0.75

The final ranking becomes: Section 2 (0.833), Section 6 (0.75), Section 7 (0.583). This makes intuitive sense - Section 2 performed well in both searches, Section 6 had mixed results, and Section 7 ranked lower overall.

Implementation

The Retriever class implementation is straightforward:

The merge logic tracks document ranks across all search results, calculates RRF scores, and returns the top-k documents sorted by their combined scores.

Testing the Hybrid Approach

When testing with the query "what happened with INC-2023-Q4-011?", the hybrid approach delivers much better results than vector search alone:


> *(See full lesson at course URL)*

#### 42. Reranking results

The hybrid retrieval approach we've built works well, but it still has some rough edges. When we search for "what did the eng team do with INC-2023-Q4-011?", we'd expect the Software Engineering section to rank higher since it specifically mentions the engineering team and the incident. However, the Cybersecurity section still comes first.

This is where re-ranking comes in - a post-processing technique that can significantly improve retrieval accuracy.

How Re-ranking Works

Re-ranking adds an extra step after your hybrid search process. Instead of just returning the merged results from your vector and BM25 indexes, you pass those results through an LLM for intelligent reordering.

The process is straightforward:

Run your existing hybrid search (vector + BM25)

Merge the results as before

Send the merged results to Claude with a re-ranking prompt

Get back a reordered list of the most relevant documents

The Re-ranking Prompt

The prompt structure is simple but effective. You provide Claude with the user's question and all the candidate documents, then ask it to return the most relevant ones in order of decreasing relevance.

Efficiency Considerations

A key optimization is using document IDs instead of asking Claude to return full text chunks. If you asked Claude to return the complete text of each relevant document, you'd waste time waiting for it to copy large amounts of text.

Instead, assign each text chunk a unique ID ahead of time, then ask Claude to return just those IDs in the preferred order. This makes the re-ranking process much faster while still giving you the reordered results you need.

Implementation

The re-ranker function gets called automatically after your initial hybrid search completes. Here's the basic structure:

You can integrate this into your retriever by passing the re-ranker function as a parameter:

Results

The re-ranking approach shows clear improvements. When testing the query "what did the eng team do with INC-2023-Q4-011?", the Software Engineering section now correctly appears first, ahead of the Cybersecurity section. Claude successfully identified that the user was specifically asking about the engineering team's involvement with the incident.

Trade-offs

Re-ranking comes with trade-offs to consider:

Increased latency: You now need to wait for an additional LLM call to complete

Improved accuracy: The LLM can understand context and intent better than pure similarity scores


> *(See full lesson at course URL)*

#### 43. Contextual retrieval

Contextual retrieval is a technique that improves RAG pipeline accuracy by solving a fundamental problem: when you split a document into chunks, each chunk loses its connection to the broader document context.

The Problem with Standard Chunking

When you take a source document and break it into chunks for your vector database, each individual piece no longer knows where it came from or how it relates to the rest of the document. This can hurt retrieval accuracy because the chunks lack important contextual information.

How Contextual Retrieval Works

Contextual retrieval adds a preprocessing step before inserting chunks into your retriever database. Here's the process:

Take each individual chunk and the original source document

Send both to Claude with a specific prompt asking it to add context

Claude generates a short snippet that "situates" the chunk within the larger document

Combine this context with the original chunk to create a "contextualized chunk"

Use the contextualized chunk in your vector and BM25 indexes

For example, if you have a section about software engineering that mentions a 2023 incident, Claude might generate context like: "This section is from a larger report about a cross-discipline group. It includes mention of INC-2023-04-011, which is also mentioned in the Cybersecurity Analysis section."

Handling Large Documents

A common problem is when your source document is too large to fit into Claude's context window. You can still use contextual retrieval by providing a reduced set of context:

Instead of including the entire document, provide:

A few chunks from the start of the document (often containing summaries or abstracts)

Chunks immediately before the chunk you're contextualizing

This approach gives Claude enough information to understand the document structure and immediate context without overwhelming the prompt.

Implementation Example

Here's a basic function for adding context to chunks:

For large documents, you can implement a strategy that selects relevant context chunks:

When to Use Contextual Retrieval

This technique is most valuable when:

Your documents have complex internal relationships between sections

Chunks reference concepts defined elsewhere in the document

Understanding the document structure is important for accurate retrieval

You're working with technical documents, reports, or academic papers


> *(See full lesson at course URL)*

#### 44. Extended thinking

Extended thinking is Claude's advanced reasoning feature that gives the model time to think through complex problems before generating a response. When enabled, Claude produces a visible thinking process that users can examine to understand how the model approached their query.

This feature significantly improves Claude's ability to handle complex tasks with greater accuracy, but it comes with important trade-offs. You'll be charged for all tokens generated during the thinking phase, and the additional processing time increases response latency. The key is knowing when the improved intelligence justifies the extra cost and wait time.

When to Use Extended Thinking

The decision to enable extended thinking should be driven by your prompt evaluations. Here's the recommended approach:

Write and test your prompt without extended thinking first

Run evaluations to measure accuracy

If results aren't meeting your standards after prompt optimization efforts

Then consider enabling extended thinking as a solution

How Extended Thinking Changes Responses

Without extended thinking, Claude's response flow is straightforward - you send a user message with a text block and receive an assistant message with a text block in return.

With extended thinking enabled, the response structure changes significantly. You'll receive an assistant message containing two distinct blocks:

A thinking block containing Claude's reasoning process

A text block with the final response

The Signature System

Each thinking block includes a cryptographic signature that serves an important security purpose. This signature ensures that the thinking text hasn't been modified when you include the message in future conversation turns.

Claude relies heavily on the thinking content for response generation, so preventing tampering is crucial for maintaining safe and consistent behavior. If you modify the thinking text, the signature validation will fail.

Redacted Thinking

Sometimes Claude's thinking process gets flagged by internal safety systems. When this happens, you'll receive a redacted thinking block instead of the raw thinking text.

The redacted content contains the actual thinking text in encrypted form. While you can't read it, you can still include this block in future conversation turns so Claude doesn't lose context from its previous reasoning.

Implementation

To enable extended thinking in your code, you'll need to modify your chat function with two new parameters:


> *(See full lesson at course URL)*

#### 45. Image support

Claude's vision capabilities let you include images in your messages and ask Claude to analyze them in sophisticated ways. You can ask Claude to describe image contents, compare multiple images, count objects, or perform complex visual analysis tasks.

Image Handling Basics

When working with images in Claude, you need to understand several key limitations:

Up to 100 images across all messages in a single request

Max size of 5MB per image

When sending one image: max height/width of 8000px

When sending multiple images: max height/width of 2000px

Images can be included as base64 encoding or a URL to the image

Each image counts as tokens based on dimensions: tokens = (width px × height px) / 750

To include an image, you add an image block to your user message alongside text blocks.

Message Flow

The conversation works just like text-only interactions. Your server sends a user message containing both image and text blocks to Claude, and Claude responds with a text message analyzing the image.

Prompting Techniques

The most important thing to understand about Claude's vision capabilities is that good prompting techniques are absolutely critical. Simple prompts often produce poor results, even with clear images.

For example, asking "How many marbles are in this image?" with an image containing 12 marbles might return an incorrect count of 13. You can dramatically improve accuracy by applying the same prompting engineering techniques you'd use for text:

Providing detailed guidelines and analysis steps

Using one-shot or multi-shot examples

Breaking down complex tasks into smaller steps

Step-by-Step Analysis

Instead of a simple question, provide Claude with a methodology. This structured approach helps Claude get correct counts by following a systematic process.

One-Shot Examples

You can also use one-shot prompting by including multiple image-text pairs in a single message. Providing an example significantly improves Claude's accuracy on the target image.

Real-World Example: Fire Risk Assessment

Here's a practical application: automating fire risk assessments for home insurance. Insurance companies often require homeowners to trim trees around their property to reduce wildfire risk. Instead of sending inspectors to each property, you can use satellite imagery with Claude.

The system analyzes satellite images to identify:

Dense, close-packed trees near the residence

Difficult access routes for emergency services


> *(See full lesson at course URL)*

#### 46. PDF support

PDF support allows Claude to analyze and extract information from PDF documents. PDFs are treated as document sources similar to plain text, enabling Claude to answer questions and provide insights based on the PDF content.

#### 47. Citations

When Claude answers questions based on documents you provide, users might assume it's just pulling information from its training data. But what if Claude is actually citing specific sources? The citations feature lets you show users exactly where Claude found its information, building trust and transparency into your AI applications.

Why Citations Matter

Without citations, users see Claude's responses as coming from memory. They have no way to verify the information or understand that it's based on specific documents you provided. Citations solve this by showing users the exact source material Claude used to generate each part of its response.

Enabling Citations

To enable citations, add two fields to your document message. The title field gives your document a name that appears in citations. The citations field with enabled: True tells Claude to track where it finds information.

Citation Structure

When citations are enabled, Claude's response becomes more complex. Instead of simple text, you get structured content with citation information.

Each citation contains:

cited_text - The exact text Claude is referencing from your document

document_index - Which document (if you provided multiple)

document_title - The title you assigned to the document

start_page_number - Where the cited text begins

end_page_number - Where the cited text ends

Building Citation Interfaces

The real power of citations comes from building user interfaces that display them. You can create numbered references in the text that link to detailed citation information. When users hover over or click citation numbers, they see exactly which document and pages Claude referenced. This transparency helps users verify information and builds confidence in Claude's responses.

Citations with Plain Text

Citations aren't limited to PDFs. You can also use them with plain text documents. With plain text, you get CitationCharLocation objects instead of page locations. These provide character positions within the text, allowing you to highlight the exact sentences or paragraphs Claude referenced.

When to Use Citations

Citations are essential when:

Users need to verify information accuracy

You're working with sensitive or important documents

Transparency about sources builds trust in your application

Users might want to read the original source material


> *(See full lesson at course URL)*

#### 48. Prompt caching

Prompt caching is a feature that speeds up Claude's responses and reduces the cost of text generation by reusing computational work from previous requests. Instead of throwing away all the processing work after each request, Claude can save and reuse it when you send similar content again.

How Claude Normally Processes Requests

To understand prompt caching, let's first look at what happens during a typical request without caching enabled.

When you send a message to Claude, it doesn't immediately start generating a response. Instead, Claude performs extensive preprocessing work on your input:

Tokenizes the prompt (breaks text into smaller units)

Creates embeddings for each token (mathematical representations)

Adds context based on surrounding text

Only then generates the actual output text

After sending you the response, Claude discards all this computational work. Everything gets thrown away, and Claude declares itself ready for the next request.

The Problem with Repeated Content

Here's where things get inefficient. Imagine you're having a conversation with Claude, so your follow-up request includes:

The same original user message from before

Claude's previous response

Your new follow-up message

Claude has to reprocess that original message all over again, even though it just analyzed the exact same content moments earlier.

How Prompt Caching Solves This

Prompt caching changes this wasteful process. Instead of discarding the preprocessing work, Claude saves it in a cache.

Here's how it works:

Initial request: Claude processes your message and writes the computational work to a cache

Follow-up requests: When Claude sees the same content again, it reads the previously processed work from the cache instead of starting over

The cache acts like a lookup table: "If I ever see this message again, I'll reuse this work I already did."

Key Benefits and Limitations

Prompt caching offers several advantages:

Faster responses: Requests using cached content execute more quickly

Lower costs: You pay less for processing that reuses cached work

Automatic optimization: The initial request writes to cache, follow-up requests read from it

However, there are important limitations to keep in mind:

Short lifespan: Cache only lives for 5 minutes

Exact matches required: Only useful when you're repeatedly sending the same content

Common use case: This happens extremely frequently in conversational applications and document analysis workflows


> *(See full lesson at course URL)*

#### 49. Rules of prompt caching

Prompt caching in Claude works by storing the computational work done on messages so it can be reused in follow-up requests. This makes requests that use cached content both cheaper and faster to execute.

The process follows a simple pattern: your initial request will write to the cache, and follow-up requests can read from the cache. The cache lives for 5 minutes, so this feature is only useful if you're repeatedly sending the same content - but this happens extremely frequently in real applications.

Cache Breakpoints

Work done on messages is not cached automatically. We have to manually add a 'cache breakpoint' to a block. Work done for everything before the breakpoint will be cached, and the cache will only be used on follow-up requests if the content up to and including the breakpoint is identical.

When you need to add cache breakpoints, you must use the longhand form for writing text blocks instead of the shorthand. Here's the difference:

Shorthand - can't add cache breakpoints

Longhand - required for cache breakpoints

How Cache Breakpoints Work

Cache breakpoints span messages and can cache assistant messages too. When you place a breakpoint, everything up to that point gets cached. Remember, content must be identical to use the cache!

In a follow-up request, Claude reads the previously processed work from the cache instead of reprocessing it.

Breakpoint Location

You're not restricted to text blocks! You can add cache breakpoints to system prompts and tool definitions. These are actually the most common caching opportunities since they rarely change between requests.

Behind the scenes, tools, system prompts, and messages get joined together in that specific order when fed into Claude. This affects how your cache breakpoints work.

You can add up to four cache breakpoints total. If you place a breakpoint on your last tool, everything up to that tool gets cached, but the system prompt and messages won't be. This gives you fine-grained control over what gets cached based on what changes in your application.

Minimum Content Length

Content must be at least 1024 tokens long to be cached (sum of all messages/blocks you're trying to cache). A simple "Hi there!" message won't meet this threshold, but if you duplicate that text 500 times, you'll have enough tokens to cache.


> *(See full lesson at course URL)*

#### 50. Prompt caching in action

Prompt caching is a powerful optimization feature that makes requests cheaper and faster when you're repeatedly sending the same content to Claude. The initial request writes to the cache, and follow-up requests can read from it. The cache lives for 5 minutes and is extremely useful since many applications send identical tool schemas, system prompts, or message histories repeatedly.

How Prompt Caching Works

When you mark content for caching, Claude processes it once and stores the result. Subsequent requests that include the exact same content can skip the processing step and read directly from the cache. This only works if the cached content is identical - even a single character change invalidates the cache.

You can set multiple cache breakpoints in a single request. The caching order follows this sequence:

Tool schemas

System prompt

Message history

Setting Up Tool Schema Caching

To cache tool schemas, you need to add a cache_control field to the last tool in your list. Here's the proper way to do it without modifying your original tool schemas:

This approach creates copies of both the tools list and the last tool schema before adding the cache control field. This prevents accidentally modifying your original tool definitions, which could cause issues if you reorder tools later.

System Prompt Caching

For system prompts, you need to structure the system parameter as a list with a text block that includes the cache control field.

Understanding Cache Behavior

When you make your first request with cacheable content, you'll see cache_creation_input_tokens in the usage field. This shows how many tokens Claude wrote to the cache. On subsequent requests with identical content, you'll see cache_read_input_tokens instead.

If you have both cached and new content in the same request, you might see both cache reads and cache writes. For example, if you keep the same tool schemas but change the system prompt, you'll read the tools from cache while writing the new system prompt to cache.

Cache Invalidation

The cache is extremely sensitive to changes. Modifying even a single character in your tool schema description, system prompt, or any cached content will invalidate that cache entry. When this happens, Claude treats it as completely new content and creates a fresh cache entry.


> *(See full lesson at course URL)*

#### 51. Introducing MCP

Model Context Protocol (MCP) is a communication layer that provides Claude with context and tools without requiring you to write a bunch of tedious integration code. Instead of building every tool function yourself, MCP shifts that burden to specialized servers that handle the heavy lifting.

When you first encounter MCP, you'll see diagrams showing the basic architecture: an MCP Client (your server) connects to MCP Servers that contain tools, prompts, and resources. Each MCP Server acts as an interface to outside services like GitHub, AWS, or databases.

The Problem MCP Solves

Let's say you're building a chat interface where users can ask Claude about their GitHub data. A user might ask "What open pull requests are there across all my repositories?" To answer this, Claude needs tools that can access GitHub's API.

GitHub has massive functionality - repositories, pull requests, issues, projects, and much more. To handle all of GitHub's features, you'd need to create an incredible number of tool schemas and functions.

This means writing, testing, and maintaining a lot of code for functions like get_repos(), list_repos(), create_repos(), search_issues(), update_issue(), create_issue(), get_issue(), create_file(), and many more.

How MCP Changes This

MCP shifts the burden of tool definitions and execution from your server to MCP Servers. Instead of you writing all those GitHub integration tools, someone else creates an MCP Server for GitHub that contains all the necessary tools and functions.

The MCP Server acts as a wrapper around the outside service, providing pre-built tools that you can use immediately. Your server becomes an MCP Client that connects to these specialized servers.

Who Creates MCP Servers

Anyone can create an MCP Server implementation. Often, service providers themselves will create official MCP implementations. For example, AWS might release their own official MCP Server with tools for their various services.

You can also create your own MCP Server to wrap access to any service you need to integrate with.

Common Questions

How is using an MCP Server different from calling a service's API directly? MCP Servers provide tool schemas and functions already defined for you. If you call an API directly, you'll be writing those tool definitions yourself. MCP saves you that implementation work.


> *(See full lesson at course URL)*

#### 52. MCP clients

The MCP client serves as the communication bridge between your server and MCP servers. Think of it as your access point to all the tools that an MCP server provides. When you need to use external functionality, the client handles all the message passing and protocol details for you.

Transport Agnostic Communication

One of MCP's key strengths is being transport agnostic - a fancy way of saying the client and server can talk to each other using different communication methods. The most common setup runs both the MCP client and server on the same machine, where they communicate through standard input/output.

But you're not limited to that approach. MCP clients and servers can also connect over HTTP, WebSockets, and various other network protocols.

Message Types

Once connected, the client and server exchange specific message types defined in the MCP specification. The main ones you'll work with are:

ListToolsRequest/ListToolsResult: The client asks the server "what tools do you provide?" and gets back a complete list of available functionality.

CallToolRequest/CallToolResult: The client tells the server "run this specific tool with these arguments" and receives the execution results.

Real-World Example Flow

Let's walk through a complete example to see how all these pieces work together. Imagine a user asks "What repositories do I have?" - here's the entire communication chain:

The process starts when a user submits their question to your server. Your server realizes it needs to provide Claude with available tools before making the AI request.

Your server asks the MCP client for a tool list, which triggers a ListToolsRequest to the MCP server. The server responds with ListToolsResult containing all available tools.

Now your server has everything needed to make the initial Claude request: the user's question plus the available tools. Claude analyzes the tools and decides it needs to call one to answer the question properly.

Claude responds with a tool use request. Your server recognizes this and asks the MCP client to execute the tool with Claude's specified arguments.

The MCP client sends a CallToolRequest to the MCP server, which then makes the actual API call to GitHub to fetch the user's repositories.

GitHub returns the repository data, which the MCP server wraps in a CallToolResult and sends back through the chain. Your server receives this data and can now make a follow-up request to Claude.


> *(See full lesson at course URL)*

#### 53. Project setup

We're going to build a CLI-based chatbot that demonstrates how MCP clients and servers work together. This hands-on project will give you practical experience with both sides of the MCP architecture.

What We're Building

Our chatbot will allow users to interact with a collection of documents through natural language. The system consists of two main components:

An MCP client that handles user interactions and communicates with Claude

An MCP server that provides tools for reading and updating documents

The server will expose two tools to Claude: a tool to read a document's contents and a tool to update a document's contents.

All documents are stored in memory for simplicity - they include files like document.pdf, spreadsheet.xlsx, report.txt, and spec.md.

Important Architecture Note

In real-world projects, you typically implement either an MCP client or an MCP server, not both. You might build just an MCP server to expose your service's capabilities to AI models, or just an MCP client to connect to existing MCP servers built by other developers.

We're building both components in this project purely for educational purposes - to understand how they communicate and work together.

Project Setup

Download the CLI_project.zip file attached to this video and extract it to your preferred development directory. Open your code editor in the project folder.

Configuration

The project includes a README.md file with detailed setup instructions. You'll need to add your Anthropic API key to the .env file and install dependencies using either UV (recommended) or pip.

The .env file should contain: ANTHROPIC_API_KEY="your-api-key-here"

Running the Project

Once setup is complete, navigate to your project directory in the terminal and run: uv run main.py (or python main.py if using standard Python).

You should see a chat prompt appear. Test it by asking a simple question like "what's 1+1?" to verify everything is working correctly.

The starter project already includes basic chat functionality with Claude. In the following videos, we'll add MCP server capabilities and document management features to create a fully functional document-aware chatbot.

#### 54. Defining tools with MCP

Building an MCP server becomes much simpler when you use the official Python SDK. Instead of writing complex JSON schemas by hand, you can define tools with decorators and let the SDK handle the heavy lifting.

In this example, we're creating a document management server with two core tools: one to read documents and another to update them. All documents exist in memory as a simple dictionary where keys are document IDs and values are the content.

Setting Up the MCP Server

The Python MCP SDK makes server creation straightforward. You can initialize a server with just one line: from mcp.server.fastmcp import FastMCP mcp = FastMCP("DocumentMCP", log_level="ERROR")

This creates a fully functional MCP server that can handle tool definitions, client connections, and message routing.

Tool Definition with Decorators

The SDK's decorator approach eliminates the need for manual JSON schema writing. Here's how you define a simple tool:

@mcp.tool( name="add_ints", description="Add two integers together", ) def tool_fn( a=Field(description="First number to add"), b=Field(description="Second number to add"), ) -> int: return a + b

Behind the scenes, MCP generates the complete tool schema that Claude needs to understand when and how to use your tool.

Building the Document Reader Tool

The first tool reads document contents by ID. It takes a document identifier and returns the corresponding content from our in-memory dictionary.

The function includes basic error handling to catch requests for non-existent documents. When Claude calls this tool with a valid document ID, it receives the full document content as a string.

Creating the Document Editor Tool

The second tool performs simple find-and-replace operations on documents. It requires three parameters: the document ID, the text to find, and the replacement text.

This implementation uses Python's built-in string replace method, which requires exact matches including whitespace. The tool modifies the document in place within our dictionary.

Key Benefits of the SDK Approach:

No manual JSON schema writing required

Type hints provide automatic parameter validation

Field descriptions help Claude understand tool usage

Error handling integrates naturally with Python exceptions

Tool registration happens automatically through decorators


> *(See full lesson at course URL)*


### 📖 Model Context Protocol

#### 55. The server inspector

The MCP Inspector is a browser-based tool that lets you test and debug your MCP server without writing any client code. Start your server with: uv run mcp dev mcp_server.py. The inspector connects to your server and shows you tools, resources, and prompts. You can click on any item to test it directly. For tools, the inspector shows input parameters and lets you fill them in. For resources, it shows the data returned. For prompts, it displays the prompt template and lets you customize variables. The inspector is invaluable for debugging - if something works in the inspector but not in your client code, you know the issue is in your client implementation.

#### 56. Implementing a client

Now that we have our MCP server working, it's time to build the client side. The client is what allows our application to communicate with the MCP server and access its functionality.

Understanding the Client Architecture
Before diving into the code, let's clarify an important point about MCP projects. Normally, you'd implement either an MCP client or an MCP server - not both. We're building both in this project just so you can see how they work together.

The MCP client consists of two main components:
- MCP Client - A custom class we create to make using the session easier
- Client Session - The actual connection to the server (part of the MCP Python SDK)

The client session requires resource cleanup when we're done with it, which is why we wrap it in our custom class. This handles connection management and cleanup automatically.

Implementing Core Client Functions
We need to implement two essential functions: list_tools and call_tool.

List Tools Function:
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools

Call Tool Function:
async def call_tool(self, tool_name: str, tool_input: dict) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)

Testing the Client
Running uv run mcp_client.py should return a list of available tools with their descriptions and input schemas.

#### 57. Defining resources

Resources in MCP servers allow you to expose data to clients, similar to GET request handlers in a typical HTTP server. They're perfect for scenarios where you need to fetch information rather than perform actions.

Understanding Resources
Think of resources as read-only endpoints that can return any type of data - strings, JSON, binary files, etc. You set a 'mime_type' to give the client a hint about what kind of data you're returning.

Two Types of Resources:
- Direct Resources: Have static URIs that don't contain any parameters (like "docs://documents")
- Templated Resources: Include parameters in their URIs that get parsed and passed to your function (like "docs://documents/{doc_id}")

Creating Resources:
@mcp.resource("docs://documents", mime_type="application/json")
def list_docs() -> list[str]:
    return list(docs.keys())

@mcp.resource("docs://documents/{doc_id}", mime_type="text/plain")
def fetch_doc(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    return docs[doc_id]

The MCP Python SDK automatically serializes whatever you return. You don't need to manually convert data to JSON strings.

#### 58. Accessing resources

Resources in MCP allow your server to expose data that can be directly included in prompts, rather than requiring tool calls to access information. This creates a more efficient way to provide context to AI models like Claude.

Understanding the Resource Flow:
When a user wants to access resource content:
1. User requests information about a resource (like "@report.pdf")
2. Your code needs a list of document names for autocomplete
3. MCP Client sends a ReadResourceRequest to the MCP Server
4. Server responds with a ReadResourceResult containing the resource data
5. Your code can then put this data directly into prompts

Implementing Resource Reading:
To read resources from your MCP client, implement a read_resource function:

import json
from pydantic import AnyUrl

async def read_resource(self, uri: str) -> Any:
    result = await self.session().read_resource(AnyUrl(uri))
    resource = result.contents[0]

Handling Different Resource Types:
if isinstance(resource, types.TextResourceContents):
    if resource.mimeType == "application/json":
        return json.loads(resource.text)
    return resource.text

Resources are particularly useful when:
- You have static or semi-static content that's frequently referenced
- You want to reduce the number of API calls
- The content should be immediately available in the prompt context

#### 59. Defining prompts

Prompts in MCP servers let you define pre-built, high-quality instructions that clients can use instead of writing their own prompts from scratch. Think of them as carefully crafted templates that give better results than what users might come up with on their own.

Why Use Prompts?
Let's say you want Claude to reformat a document into markdown. A user could just type 'convert report.pdf to markdown' and get decent results. But they'd probably get much better output if they used a thoroughly tested, specialized prompt that you've designed specifically for document formatting.

How Prompts Work:
Prompts define a set of user and assistant messages that clients can use directly. When a client requests a prompt, your server returns a list of messages that can be sent straight to Claude.

The basic structure:
@mcp.prompt(
    name="format",
    description="Rewrites the contents of a document in Markdown format",
)
def format_document(
    doc_id: str = Field(description="Id of the document to format"),
) -> list[base.Message]:
    # Return a list of messages

Building a Format Command:
def format_document(
    doc_id: str = Field(description="Id of the document to format"),
) -> list[base.Message]:
    prompt = f"""
    Your goal is to reformat a document to be written with markdown syntax.
    The id of the document you need to reformat is: {doc_id}
    Add in headers, bullet points, tables, etc as necessary.
    Feel free to add in structure.
    Use the 'edit_document' tool to edit the document.
    After the document has been reformatted...
    """
    return [ base.UserMessage(prompt) ]

Key Benefits:
- Quality control: You can test and refine prompts before users see them
- Consistency: Users get reliable results every time
- Specialization: Prompts can be tailored to your server's specific domain
- Reusability: Multiple clients can use the same well-crafted prompts

#### 60. Prompts in the client

The final step in building our MCP client is implementing prompt functionality. This allows us to list all available prompts from the server and retrieve specific prompts with variables interpolated into them.

Implementing List Prompts:
The list_prompts method is straightforward. We call the session's list prompts method and return the prompts:
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts

Getting Individual Prompts:
The get_prompt method handles argument interpolation. When we request a specific prompt, we pass arguments that get injected into the prompt function:

async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages

Testing the Implementation:
Once implemented, you can test prompts through the CLI. Available prompts appear as commands when you type a forward slash. Selecting a prompt lets you provide arguments and the system retrieves the prompt with your arguments interpolated, sends it to Claude, and returns the result.

#### 61. MCP review

Now that we've built our MCP server, let's review the three core primitives and understand when to use each one.

Tools: Model-Controlled:
Tools are controlled entirely by Claude. The AI model decides when to call these functions, and the results are used directly by Claude.
Use tools when you want to give Claude additional capabilities. For example, if you ask Claude to calculate the square root of 3 using JavaScript, Claude will automatically decide to use a JavaScript execution tool.

Resources: App-Controlled:
Resources are controlled by your application code. Your app decides when to fetch resource data and how to use it, typically for UI purposes or to add context to conversations.
Use resources when you need to get data into your app. Common examples include:
- Populating autocomplete options in your UI
- Adding context to messages before sending them to Claude
- Displaying lists of available documents or files

Prompts: User-Controlled:
Prompts are triggered by user actions. Users decide when to run these predefined workflows through UI interactions.
Use prompts for workflows that users should be able to trigger on demand. These are perfect for:
- Predefined conversation starters
- Common task templates
- Specialized workflows optimized for specific use cases

Choosing the Right Primitive:
- Need to extend Claude's capabilities? Use tools
- Need data for your app's UI or context? Use resources
- Want to offer predefined workflows to users? Use prompts

Remember: tools serve the model, resources serve your app, and prompts serve your users.


### 📖 Anthropic apps - Claude Code and computer use

#### 62. Claude Code setup

Claude Code is Anthropic's CLI tool for AI-powered development. This lesson covers setting up Claude Code on your machine for automated coding tasks.

Key topics include:
- Installing Claude Code via npm or package manager
- Authentication with your Anthropic API credentials
- Basic configuration and project initialization
- Running Claude Code in different modes (interactive, batch, headless)
- Integration with version control systems

Claude Code can be used for tasks like writing new code, modifying existing files, searching and navigation, and running terminal commands.

#### 63. Claude Code in action

This lesson demonstrates Claude Code in a real development workflow.

You'll see examples of:
- Starting a new project with Claude Code
- Making targeted changes to existing codebases
- Handling multi-file refactoring tasks
- Debugging and fixing issues
- Writing tests and documentation

Claude Code uses a conversational interface where you describe what you want to accomplish, and it proposes and implements changes, asking for confirmation when needed before making modifications.

#### 64. Enhancements with MCP servers

Claude Code can be extended with MCP (Model Context Protocol) servers to access external tools and data sources.

This lesson shows how to:
- Connect Claude Code to your custom MCP servers
- Configure MCP server connections in Claude Code settings
- Use tools from MCP servers within Claude Code sessions
- Build and deploy custom MCP servers for specialized functionality

MCP servers allow Claude Code to access databases, APIs, file systems, and other resources beyond what comes built-in, enabling more powerful automation workflows.

#### 65. Parallelizing Claude Code

Running multiple Claude Code sessions in parallel can dramatically speed up large projects.

This lesson covers:
- Running independent Claude Code tasks simultaneously
- Managing multiple concurrent sessions
- Handling shared resources and conflicts
- Aggregating results from parallel runs
- Best practices for parallel execution

When you have independent tasks (like working on different features or files), parallelizing lets Claude work on multiple things at once, reducing overall completion time.

#### 66. Automated debugging

Claude Code excels at debugging - it can analyze error messages, trace through code, and identify issues.

Key debugging capabilities:
- Analyzing stack traces and error output
- Reading and understanding code logic
- Proposing and testing fixes
- Running tests to verify corrections
- Explaining what went wrong and why

You can feed Claude Code error logs, crash reports, or describe unexpected behavior, and it will investigate the codebase to find and fix the root cause.

#### 67. Computer use

Computer use is Anthropic's capability that allows Claude to interact with computer interfaces directly.

This lesson introduces:
- How Claude can see and interact with screen content
- Moving the mouse and typing keyboard input
- Taking screenshots and analyzing visual information
- Navigating applications and websites
- Limitations and safety considerations

Computer use enables Claude to automate tasks that require interacting with GUI applications, websites, or desktop software - anything a human can do with a keyboard and mouse.

#### 68. How computer use works

Under the hood, computer use combines several capabilities:

1. Screenshot capture - Claude takes periodic screenshots of the screen
2. Vision processing - Images are analyzed using Claude's vision capabilities
3. Action prediction - Claude decides what mouse/keyboard actions to take
4. Execution - Actions are performed via automated tools
5. Feedback loop - New screenshots confirm the result

The API sends screenshots to Claude along with available actions, and Claude responds with which action to take. This creates a closed loop where Claude can accomplish complex UI automation tasks.

Safety features include confirmation prompts for destructive actions and rate limiting to prevent runaway automation.


### 📖 Agents and workflows

#### 69. Agents and workflows

This section introduces advanced patterns for building complex AI applications with Claude.

Key concepts:
- Agents: Autonomous entities that can use tools and make decisions
- Workflows: Predefined patterns for combining AI capabilities
- Orchestration: Managing multiple AI components working together

You'll learn about different workflow patterns including:
- Parallelization: Running multiple tasks simultaneously
- Chaining: Passing output from one step as input to the next
- Routing: Directing requests to different handlers based on conditions
- Agents: Giving Claude goal-directed autonomy with tool use

These patterns enable building sophisticated AI systems that go beyond single API calls.

#### 70. Parallelization workflows

Parallelization lets you run multiple AI operations concurrently, improving throughput.

When to use parallelization:
- Processing multiple independent requests at once
- Running the same operation on different inputs
- Fetching multiple data sources simultaneously
- Performing analysis on separate document chunks

Implementation approaches:
1. Batch API: Send multiple requests in a single API call where supported
2. Async/await: Use asynchronous programming to run requests concurrently
3. Thread pools: Manage concurrent workers for CPU-bound tasks
4. Message queues: Decouple request handling for scalability

Considerations:
- Rate limits may restrict concurrent requests
- Error handling becomes more complex
- Costs can accumulate quickly with parallel usage

#### 71. Chaining workflows

Chaining connects multiple AI operations where each step's output feeds into the next.

Common patterns:
1. Generate-Then-Refine: Create initial content, then improve it
2. Analyze-Then-Summarize: Extract key info, then create a digest
3. Search-Then-Answer: Find relevant documents, then answer the question
4. Translate-Then-Review: Translate content, then have AI review for accuracy

Implementation:
response1 = client.messages.create(prompt=initial_prompt)
response2 = client.messages.create(prompt=f"Based on: {response1.content}, {next_prompt}")

Benefits:
- Break complex tasks into manageable steps
- Improve quality through iterative refinement
- Handle tasks too long for single context windows
- Build sophisticated pipelines from simple components

#### 72. Routing workflows

Routing directs requests to different handlers based on analysis of the input.

How routing works:
1. Analyze incoming request to understand intent
2. Classify or categorize the request type
3. Route to appropriate handler or specialist AI
4. Handler processes and returns result

Use cases:
- Customer service: Route to different departments based on issue type
- Content moderation: Send to appropriate review pipeline
- Document processing: Direct to specialized extractors by document type
- Skill routing: Send to AIs specialized in different domains

Implementation options:
- Rule-based: Simple if/else logic for clear categories
- Embedding-based: Use vector similarity for fuzzy matching
- LLM-based: Use AI to interpret and route based on understanding

#### 73. Agents and tools

Agents combine AI reasoning with tool use to accomplish complex, multi-step goals autonomously.

Agent architecture:
1. Core AI model that decides actions
2. Available tools with descriptions
3. Working memory to track state
4. Loop: think -> act -> observe -> repeat

Key capabilities:
- Autonomous decision-making without human intervention per step
- Maintaining context across multiple tool calls
- Handling failures and trying alternative approaches
- Knowing when to stop and return results

Building an agent involves:
- Defining the agent's goal and success criteria
- Providing relevant tools for the domain
- Setting appropriate boundaries and constraints
- Implementing the agent loop with state management

#### 74. Environment inspection

Before taking actions, agents need to understand their environment.

Environment inspection involves:
- Reading files to understand codebase structure
- Checking available resources and permissions
- Querying APIs or databases for current state
- Examining configuration and environment variables
- Understanding dependencies and relationships

Tools for inspection:
- File system tools: read, list, search files
- Command execution: run shell commands to gather info
- API clients: query external services for state
- Database connectors: inspect data stores

Best practices:
- Inspect before acting to make informed decisions
- Cache inspection results to avoid redundant calls
- Handle missing or inaccessible resources gracefully

#### 75. Workflows vs agents

Understanding when to use workflows vs agents is key to building effective AI systems.

Workflows are best for:
- Predictable, sequential processes
- When you need to enforce specific steps
- Compliance-required audit trails
- Fixed pipelines with known inputs/outputs
- When determinism matters

Agents are best for:
- Open-ended problem solving
- When the path to the goal is unknown
- Dynamically adapting to results
- Complex reasoning with tool use
- When flexibility outweighs predictability

Hybrid approaches:
Use workflows to orchestrate multiple agents, where each agent handles a specialized sub-task. This gives you the flexibility of agents within the structure of workflows.

Decision framework:
If you can define the exact steps -> Use workflows
If the AI needs to figure out the steps -> Use agents

#### 76. Quiz on agents and workflows



### 📖 Final assessment

#### 77. Final assessment quiz



### 📖 Wrapping up!

#### 78. Course Wrap Up

Congratulations on completing the Claude with Google Cloud's Vertex AI course!

What you've learned:
- Setting up and configuring Claude models via Vertex AI
- Implementing multi-turn conversations with proper context management
- Designing and evaluating prompts with systematic testing
- Building tool-use implementations for external integrations
- Developing RAG pipelines with embeddings and search
- Using advanced Claude features: vision, PDFs, citations, caching
- Implementing MCP servers for tool, resource, and prompt primitives
- Deploying Claude Code and Computer Use for automation
- Designing agent-based workflows with advanced orchestration patterns

Next steps:
- Apply these skills to your own projects
- Experiment with different patterns covered in the course
- Explore Anthropic's documentation for deeper dives
- Join the developer community to share experiences

Thank you for taking this course!


---

*Summary generated from course content at https://anthropic.skilljar.com/claude-with-google-vertex*
