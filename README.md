# SEMANTIC_KERNEL

![licence](https://img.shields.io/badge/licence-MIT-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![checks](https://img.shields.io/badge/checks-unknown_PASS-brightgreen)

> Governed Anticloud packaging of the upstream project `SEMANTIC_KERNEL` in category **FRONTIER_HARNESSES**. check results: see ISOLATED_LAB_RESULTS. Every number below traces to a named file + run stamp; nothing is borrowed from other projects.

**Upstream:** https://github.com/microsoft/semantic-kernel · **Upstream pin:** `cc8a15fa356f02dcb7bc64999392ca02f3167312` · **Category:** FRONTIER_HARNESSES · **Vendor:** Anticloud FZ LLE · **Licence:** MIT

---

## What This Project Does

# Semantic Kernel

> [!IMPORTANT]
> Semantic Kernel is now [Microsoft Agent Framework](https://github.com/microsoft/agent-framework)! Microsoft Agent Framework (MAF) is the enterprise‑ready successor to Semantic Kernel. Microsoft Agent Framework is now available at version 1.0 as a production-ready release: stable APIs, and a commitment to long-term support. Whether you're building a single assistant or orchestrating a fleet of specialized agents, Microsoft Agent Framework 1.0 gives you enterprise-grade multi-agent orchestration, multi-provider model support, and cross-runtime interoperability via A2A and MCP.
>
> Learn more about Semantic Kernel and Agent Framework here: [Semantic Kernel and Microsoft Agent Framework on the Agent Framework blog](https://devblogs.microsoft.com/agent-framework/semantic-kernel-and-microsoft-agent-framework/), and try out the [Semantic Kernel migration guide](https://learn.microsoft.com/en-us/agent-framework/migration-guide/from-semantic-kernel).

**Build intelligent AI agents and multi-agent systems with this enterprise-ready orchestration framework**

[![License: MIT](https://img.shields.io/github/license/microsoft/semantic-kernel)](https://github.com/microsoft/semantic-kernel/blob/main/LICENSE)
[![Python package](https://img.shields.io/pypi/v/semantic-kernel)](https://pypi.org/project/semantic-kernel/)
[![Nuget package](https://img.shields.io/nuget/vpre/Microsoft.SemanticKernel)](https://www.nuget.org/packages/Microsoft.SemanticKernel/)
[![Discord](https://img.shields.io/discord/1063152441819942922?label=Discord&logo=discord&logoColor=white&color=d82679)](https://aka.ms/SKDiscord)

## What is Semantic Kernel?

Semantic Kernel is a model-agnostic SDK that empowers developers to build, orchestrate, and deploy AI agents and multi-agent systems. Whether you're building a simple chatbot or a complex multi-agent workflow, Semantic Kernel provides the tools you need with enterprise-grade reliability and flexibility.

## System Requirements

- **Python**: 3.10+
- **.NET**: .NET 10.0+ 
- **Java**: JDK 17+
- **OS Support**: Windows, macOS, Linux

## Key Features

- **Model Flexibility**: Connect to any LLM with built-in support for [OpenAI](https://platform.openai.com/docs/introduction), [Azure OpenAI](https://azure.microsoft.com/en-us/products/ai-services/openai-service), [Hugging Face](https://huggingface.co/), [NVidia](https://www.nvidia.com/en-us/ai-data-science/products/nim-microservices/) and more
- **Agent Framework**: Build modular AI agents with access to tools/plugins, memory, and planning capabilities
- **Multi-Agent Systems**: Orchestrate complex workflows with collaborating specialist agents
- **Plugin Ecosystem**: Extend with native code functions, prompt templates, OpenAPI specs, or Model Context Protocol (MCP)
- **Vector DB Support**: Seamless integration with [Azure AI Search](https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search), [Elasticsearch](https://www.elastic.co/), [Chroma](https://docs.trychroma.com/docs/overview/getting-started), and more
- **Multimodal Support**: Process text, vision, and audio inputs
- **Local Deployment**: Run with [Ollama](https://ollama.com/), [LMStudio](https://lmstudio.ai/), or [ONNX](https://onnx.ai/)
- **Process Framework**: Model complex business processes with a structured workflow approach
- **Enterprise Ready**: Built for observability, security, and stable APIs

## Installation

First, set the environment variable for your AI Services:

**Azure OpenAI:**
```bash
export AZURE_OPENAI_API_KEY=AAA....
```

**or OpenAI directly:**
```bash
export OPENAI_API_KEY=sk-...
```

### Python

```bash
pip install semantic-kernel
```

### .NET

```bash
dotnet add package Microsoft.SemanticKernel
dotnet add package Microsoft.SemanticKernel.Agents.Core
```

### Java

See [semantic-kernel-java build](https://github.com/microsoft/semantic-kernel-java/blob/main/BUILD.md) for instructions.

## Quickstart

### Basic Agent - Python

Create a simple assistant that responds to user prompts:

```python
import asyncio
from semantic_kernel.agents import ChatCompletionAgent
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion

async def main():
    # Initialize a chat agent with basic instructions
    agent = ChatCompletionAgent(
        service=AzureChatCompletion(),
        name="SK-Assistant",
        instructions="You are a helpful assistant.",
    )

    # Get a response to a user message
    response = await agent.get_response(messages="Write a haiku about Semantic Kernel.")
    print(response.content)

asyncio.run(main()) 

# Output:
# Language's essence,
# Semantic threads intertwine,
# Meaning's core revealed.
```

### Basic Agent - .NET

```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Agents;

var builder = Kernel.CreateBuilder();
builder.AddAzureOpenAIChatCompletion(
                Environment.GetEnvironmentVariable("AZURE_OPENAI_DEPLOYMENT"),
                Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT"),
                Environment.GetEnvironmentVariable("AZURE_OPENAI_API_KEY")
                );
var kernel = builder.Build();

ChatCompletionAgent agent =
    new()
    {
        Name = "SK-Agent",
        Instructions = "You are a helpful assistant.",
        Kernel = kernel,
    };

await foreach (AgentResponseItem<ChatMessageContent> response 
    in agent.InvokeAsync("Write a haiku about Semantic Kernel."))
{
    Console.WriteLine(response.Message);
}

// Output:
// Language's essence,
// Semantic threads intertwine,
// Meaning's core revealed.
```

### Agent with Plugins - Python

Enhance your agent with custom tools (plugins) and structured output:

```python
import asyncio
from typing import Annotated
from pydantic import BaseModel
from semantic_kernel.agents import ChatCompletionAgent
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion, OpenAIChatPromptExecutionSettings
from semantic_kernel.functions import kernel_function, KernelArguments

class MenuPlugin:
    @kernel_function(description="Provides a list of specials from the menu.")
    def get_specials(self) -> Annotated[str, "Returns the specials from the menu."]:
        return """
        Special Soup: Clam Chowder
        Special Salad: Cobb Salad
        Special Drink: Chai Tea
        """

    @kernel_function(description="Provides the price of the requested menu item.")
    def get_item_price(
        self, menu_item: Annotated[str, "The name of the menu item."]
    ) -> Annotated[str, "Returns the price of the menu item."]:
        return "$9.99"

class MenuItem(BaseModel):
    price: float
    name: str

async def main():
    # Configure structured output format
    settings = OpenAIChatPromptExecutionSettings()
    settings.response_format = MenuItem

    # Create agent with plugin and settings
    agent = ChatCompletionAgent(
        service=AzureChatCompletion(),
        name="SK-Assistant",
        instructions="You are a helpful assistant.",
        plugins=[MenuPlugin()],
        arguments=KernelArguments(settings)
    )

    response = await agent.get_response(messages="What is the price of the soup special?")
    print(response.content)

    # Output:
    # The price of the Clam Chowder, which is the soup special, is $9.99.

asyncio.run(main()) 
```

### Agent with Plugin - .NET

```csharp
using System.ComponentModel;
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Agents;
using Microsoft.SemanticKernel.ChatCompletion;

var builder = Kernel.CreateBuilder();
builder.AddAzureOpenAIChatCompletion(
                Environment.GetEnvironmentVariable("AZURE_OPENAI_DEPLOYMENT"),
                Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT"),
                Environment.GetEnvironmentVariable("AZURE_OPENAI_API_KEY")
                );
var kernel = builder.Build();

kernel.Plugins.Add(KernelPluginFactory.CreateFromType<MenuPlugin>());

ChatCompletionAgent agent =
    new()
    {
        Name = "SK-Assistant",
        Instructions = "You are a helpful assistant.",
        Kernel = kernel,
        Arguments = new KernelArguments(new PromptExecutionSettings() { FunctionChoiceBehavior = FunctionChoiceBehavior.Auto() })

    };

await foreach (AgentResponseItem<ChatMessageContent> response 
    in agent.InvokeAsync("What is the price of the soup special?"))
{
    Console.WriteLine(response.Message);
}

sealed class MenuPlugin
{
    [KernelFunction, Description("Provides a list of specials from the menu.")]
    public string GetSpecials() =>
        """
        Special Soup: Clam Chowder
        Special Salad: Cobb Salad
        Special Drink: Chai Tea
        """;

    [KernelFunction, Description("Provides the price of the requested menu item.")]
    public string GetItemPrice(
        [Description("The name of the menu item.")]
        string menuItem) =>
        "$9.99";
}
```

### Multi-Agent System - Python

Build a system of specialized agents that can collaborate:

```python
import asyncio
from semantic_kernel.agents import ChatCompletionAgent, ChatHistoryAgentThread
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion, OpenAIChatCompletion

billing_agent = ChatCompletionAgent(
    service=AzureChatCompletion(), 
    name="BillingAgent", 
    instructions="You handle billing issues like charges, payment methods, cycles, fees, discrepancies, and payment failures."
)

refund_agent = ChatCompletionAgent(
    service=AzureChatCompletion(),
    name="RefundAgent",
    instructions="Assist users with refund inquiries, including eligibility, policies, processing, and status updates.",
)

triage_agent = ChatCompletionAgent(
    service=OpenAIChatCompletion(),
    name="TriageAgent",
    instructions="Evaluate user requests and forward them to BillingAgent or RefundAgent for targeted assistance."
    " Provide the full answer to the user containing any information from the agents",
    plugins=[billing_agent, refund_agent],
)

thread: ChatHistoryAgentThread = None

async def main() -> None:
    print("Welcome to the chat bot!\n  Type 'exit' to exit.\n  Try to get some billing or refund help.")
    while True:
        user_input = input("User:> ")

        if user_input.lower().strip() == "exit":
            print("\n\nExiting chat...")
            return False

        response = await triage_agent.get_response(
            messages=user_input,
            thread=thread,
        )

        if response:
            print(f"Agent :> {response}")

# Agent :> I understand that you were charged twice for your subscription last month, and I'm here to assist you with resolving this issue. Here’s what we need to do next:

# 1. **Billing Inquiry**:
#    - Please provide the email address or account number associated with your subscription, the date(s) of the charges, and the amount charged. This will allow the billing team to investigate the discrepancy in the charges.

# 2. **Refund Process**:
#    - For the refund, please confirm your subscription type and the email address associated with your account.
#    - Provide the dates and transaction IDs for the charges you believe were duplicated.

# Once we have these details, we will be able to:

# - Check your billing history for any discrepancies.
# - Confirm any duplicate charges.
# - Initiate a refund for the duplicate payment if it qualifies. The refund process usually takes 5-10 business days after approval.

# Please provide the necessary details so we can proceed with resolving this issue for you.

if __name__ == "__main__":
    asyncio.run(main())
```

## Where to Go Next

1. 📖 Try our [Getting Started Guide](https://learn.microsoft.com/en-us/semantic-kernel/get-started/quick-start-guide) or learn about [Building Agents](https://learn.microsoft.com/en-us/semantic-kernel/frameworks/agent/)
2. 🔌 Explore over 100 [Detailed Samples](https://learn.microsoft.com/en-us/semantic-kernel/get-started/detailed-samples)
3. 💡 Learn about core Semantic Kernel [Concepts](https://learn.microsoft.com/en-us/semantic-kernel/concepts/kernel)

### API References

- [C# API reference](https://learn.microsoft.com/en-us/dotnet/api/microsoft.semantickernel?view=semantic-kernel-dotnet)
- [Python API reference](https://learn.microsoft.com/en-us/python/api/semantic-kernel/semantic_kernel?view=semantic-kernel-python)

## Troubleshooting

### Common Issues

- **Authentication Errors**: Check that your API key environment variables are correctly set
- **Model Availability**: Verify your Azure OpenAI deployment or OpenAI model access

### Getting Help

- Check our [GitHub issues](https://github.com/microsoft/semantic-kernel/issues) for known problems
- Search the [Discord community](https://aka.ms/SKDiscord) for solutions
- Include your SDK version and full error messages when asking for help

## Join the community

We welcome your contributions and suggestions to the SK community! One of the easiest ways to participate is to engage in discussions in the GitHub project. Bug reports and fixes are welcome!

For new features, components, or extensions, please open an issue and discuss with us before sending a PR. This is to avoid rejection as we might be taking the core in a different direction, but also to consider the impact on the larger ecosystem.

To learn more and get started:

- Read the [documentation](https://aka.ms/sk/learn)
- Learn how to [contribute](https://learn.microsoft.com/en-us/semantic-kernel/support/contributing) to the project
- Ask questions in the [GitHub discussions](https://github.com/microsoft/semantic-kernel/discussions)
- Ask questions in the [Discord community](https://aka.ms/SKDiscord)

- Attend [regular office hours and SK community events](COMMUNITY.md)
- Follow the team on our [blog](https://aka.ms/sk/blog)

## Contributor Wall of Fame

[![semantic-kernel contributors](https://contrib.rocks/image?project=microsoft/semantic-kernel)](https://github.com/microsoft/semantic-kernel/graphs/contributors)

## Code of Conduct

This project has adopted the
[Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information, see the
[Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/)
or contact [opencode@microsoft.com](mailto:opencode@microsoft.com)
with any additional questions or comments.

## License

Copyright (c) Microsoft Corporation. All rights reserved.

Licensed under the [MIT](LICENSE) license.

---

## Installation

First, set the environment variable for your AI Services:

**Azure OpenAI:**
```bash
export AZURE_OPENAI_API_KEY=AAA....
```

**or OpenAI directly:**
```bash
export OPENAI_API_KEY=sk-...
```

## Usage

```python
import asyncio
from semantic_kernel.agents import ChatCompletionAgent
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion

async def main():
    # Initialize a chat agent with basic instructions
    agent = ChatCompletionAgent(
        service=AzureChatCompletion(),
        name="SK-Assistant",
        instructions="You are a helpful assistant.",
    )

    # Get a response to a user message
    response = await agent.get_response(messages="Write a haiku about Semantic Kernel.")
    print(response.content)

asyncio.run(main())

## API

>
> Learn more about Semantic Kernel and Agent Framework here: [Semantic Kernel and Microsoft Agent Framework on the Agent Framework blog](https://devblogs.microsoft.com/agent-framework/semantic-kernel-and-microsoft-agent-framework/), and try out the [Semantic Kernel migration guide](https://learn.microsoft.com/en-us/agent-framework/migration-guide/from-semantic-kernel).

**Build intelligent AI agents and multi-agent systems with this enterprise-ready orchestration framework**

[![License: MIT](https://img.shields.io/github/license/microsoft/semantic-kernel)](https://github.com/microsoft/semantic-kernel/blob/main/LICENSE)
[![Python package](https://img.shields.io/pypi/v/semantic-kernel)](https://pypi.org/project/semantic-kernel/)
[![Nuget package](https://img.shields.io/nuget/vpre/Microsoft.SemanticKernel)](https://www.nuget.org/packages/Microsoft.SemanticKernel/)
[![Discord](https://img.shields.io/discord/1063152441819942922?label=Discord&logo=discord&logoColor=white&color=d82679)](https://aka.ms/SKDiscord)

## Dependencies

| Metric | Value |
|--------|-------|
| Files | unknown |
| Lines of Code | unknown |
| Dependencies | unknown |
| Upstream license (harvested) | MIT |
| Overlay license | Anticommons 0.1.0 |

Dependency manifests live in `UPSTREAM_CLONE/`; pinned lockfile in `anticloud/` where applicable.

## Configuration

**Azure OpenAI:**
```bash
export AZURE_OPENAI_API_KEY=AAA....
```

**or OpenAI directly:**
```bash
export OPENAI_API_KEY=sk-...
```

## Contributing

# Contributing to Semantic Kernel

You can contribute to Semantic Kernel with issues and pull requests (PRs). Simply
filing issues for problems you encounter is a great way to contribute. Contributing
code is greatly appreciated.

## Reporting Issues

We always welcome bug reports, API proposals and overall feedback. Here are a few
tips on how you can make reporting your issue as effective as possible.

### Where to Report

New issues can be reported in our [list of issues](https://github.com/microsoft/semantic-kernel/issues).

Before filing a new issue, please search the list of issues to make sure it does
not already exist.

If you do find an existing issue for what you wanted to report, please include
your own feedback in the discussion. Do consider upvoting (👍 reaction) the original
post, as this helps us prioritize popular issues in our backlog.

### Writing a Good Bug Report

Good bug reports make it easier for maintainers to verify and root cause the
underlying problem.
The better a bug report, the faster the problem will be resolved. Ideally, a bug
report should contain the following information:

- A high-level description of the problem.
- A _minimal reproduction_, i.e. the smallest size of code/configuration required
  to reproduce the wrong behavior.
- A description of the _expected behavior_, contrasted with the _actual behavior_ observed.
- Information on the environment: OS/distribution, CPU architecture, SDK version, etc.
- Additional information, e.g. Is it a regression from previous versions? Are there
  any known workarounds?

## Contributing Changes

Project maintainers will merge accepted code changes from contributors.

### DOs and DON'Ts

DO's:

- **DO** follow the standard coding conventions

  - [.NET](https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions)
  - [Python](https://pypi.org/project/black/)
  - [Typescript](https://typescript-eslint.io/rules/)/[React](https://github.com/jsx-eslint/eslint-plugin-react/tree/master/docs/rules)

- **DO** give priority to the current style of the project or file you're changing
  if it diverges from the general guidelines.
- **DO** include tests when adding new features. When fixing bugs, start with
  adding a test that highlights how the current behavior is broken.
- **DO** keep the discussions focused. When a new or related topic comes up
  it's often better to create new issue than to side track the discussion.
- **DO** clearly state on an issue that you are going to take on implementing it.
- **DO** blog and tweet (or whatever) about your contributions, frequently!

DON'Ts:

- **DON'T** surprise us with big pull requests. Instead, file an issue and start
  a discussion so we can agree on a direction before you invest a large amount of time.
- **DON'T** commit code that you didn't write. If you find code that you think is a good
  fit to add to Semantic Kernel, file an issue and start a discussion before proceeding.
- **DON'T** submit PRs that alt

## License

Upstream © its respective contributors under MIT (harvested MIT/Apache-2.0/BSD source; see `UPSTREAM_CLONE/LICENSE`). This packaging overlay is licensed under Anticommons 0.1.0.

## Upstream

- **project:** https://github.com/microsoft/semantic-kernel
- **Pinned SHA:** `cc8a15fa356f02dcb7bc64999392ca02f3167312`
- **source:** `UPSTREAM_CLONE/` (pinned at the SHA above)
- **Upstream README source:** `UPSTREAM_CLONE/README.md`

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`16f3714783d9083cd6201b1013fcc57f46b8fc016844beaff9d11d102da80bb3`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

