# LLM Server Project - Minutes of Meeting
## Infrastructure

- LLM server: V100 DGX, 128GB — Option 1 (models not exceeding 17B parameters)
- Llama Scout 109B under consideration — suited to the nature of the work, but guardrail status unconfirmed
- Two models available for students to connect to
- VPN + port mapping required to access the model; configuration details to be provided

## Agentic Framework

- Decision: stick with **Pydantic** for the backend
    - `pydantic_ai.providers` — backend workflow, tool calling, orchestration
    - Open question: what "pydantic_vstorm deep agent" / **deepagents** refers to — needs research
    - Open question: what a "harness" is in this context — needs exploration
- Multi-agent orchestration
    - Need to define the architecture: how sub-agents are called
    - Every agent must be an expert in a specific domain
- Context management
    - How to populate context for the AI
    - How to prevent the AI from losing context
- Tools and skills
    - One person to own tool-building and verifying results
    - Tools/skills not covered by Pydantic: build in-house or source an existing repo
    - MCP tool calling / `skills.md` — can use an existing MCP server
    - Medium articles may help if needed (paid access)

## Frontend / UI

- Need a frontend for the agentic system — to observe agent actions and issue commands
- Candidate generative-UI approaches to evaluate: **AG-UI**, **A2UI**, and Pydantic AI's own Web Chat UI
- Research needed on API keys and calls for the frontend integration
- Reference: OpenAI/HuggingFace incident involving a frontier-lab model intrusion UI (shared as a cautionary example — [huggingface space link])
- Open question: what ST (the team/vendor referenced) can support on the frontend

## Open Questions

- What is vLLM?
- What support can ST provide on the frontend?
- Does Llama Scout 109B have guardrails in place?
- What is "pydantic deepagents" / "deep agent"?
- What is a "harness" in the agentic context?
- How should multi-agent orchestration be architected (sub-agent calling)?
- How is AI context best populated and preserved across a session?

## Action Items

|   |   |
|---|---|
|Item|Notes|
|Get WhatsApp contact for Stefanie|Dr. Zhao's contact, lead architect / teaching assistant|
|Research vLLM|—|
|Research pydantic deepagents / "harness" concept|—|
|Research AG-UI, A2UI, and Pydantic AI Web Chat UI|For frontend generative-UI decision|
|Research API key/call setup for frontend integration|—|
|Design multi-agent orchestration architecture|Define how sub-agents are called|
|Set up VPN and port mapping to LLM server|Configuration to be provided by team|
|Confirm guardrail status of Llama Scout 109B|—|



Meeting Date: 26 October 2026  
Attendees: Mr. Gibbsen Omar, Edbert Lee, Calissa Soh, Shi Difen(Stefanie)  
  
Notes (Items discussed):  
The team and ST has discussed on the following:  
- RP's server infrastructure and if we're using RP's server or ST Engineering server for the LLM.  
- What is the LLM model that's going to be adopted into the project.  
- The project scope, feasibility of the project and the requirements from ST.   
- Which AI framework to adopt. PydanticAI or Lang Graph/Lang Chain? Final decision was on PydanticAI.  
  
Action Items:  
- Create a Whatsapp group with ST Engineering - Created by Mr.Gibbsen.   
- Research on vLLM - Teamwork   
- Research on WebChatUI, AG-UI, A2UI for the front end of the agent dashboard - Teamwork.   
- Set up VPN and port mapping to access ST Engineering's infrastructure - ST Engineering to confirm the dates with the students (us).  
- Research on the use case and integration of the AI models and scripting - Team.   
  
Internal Meeting by the team:   
Actionable Items:   
3. To be done by Edbert:
	1. Lab to be configured - Edbert.   
	2. LFI, RCE to be designed and configured for vulnerable website. 
	3. Research and implement ASP Roasting on AD. 
4. To be done by Calissa:
	1. SQLi attacks and XSS to be designed and implemented - Calissa. 
	2. Research and implement (if possible) Pass-the-Hash attack on AD. 
5. To be done by Stefanie:
	1. IDOR and CSRF to be designed and configured for vulnerable website. 
	2. Research on the implementation of Keberoasting.