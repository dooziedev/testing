flowchart LR
    discord([Discord]) -->|msgs| bot[[Bot client]]
    bot -->|replies| discord
    bot -->|context request| context[[Context builder]]
    personality(Personality) -->|persona| context
    mood(Mood) -->|mood state| context
    memory[(Memory)] -->|recall| context
    context -->|prompt| bot
    bot -->|chat| llm[(LLM provider)]
    llm -->|response| bot
    bot -->|candidate| guard[[Delivery guard]]
    guard -->|send / regen| bot
    bot -->|log| memory
    bot -->|sentiment| mood
    autonomy(Autonomy) -->|proactive| discord
    autonomy -->|reflection| personality
    autonomy -->|goal mining| memory
    memory -->|persist| sqlite[(SQLite)]
    personality -->|snapshots| sqlite
    mood -->|mood_log| sqlite
    dashboard([Dashboard]) -->|start/stop + control| bot


flowchart TD
    %% User Input
    User([User Chat Input]) --> NLP[NLP & Sentiment Analysis]
    NLP --> Intent[Extracted Intent & Entities]
    NLP --> Sentiment[Sentiment Score]

    %% Dynamic Parameters
    subgraph Dynamic Variables
        Context[Max Context Size]
        Params[Model Parameter Scale]
        OutputLimit[Target Output Size]
    end

    %% Routing
    Intent & Sentiment --> Controller{Dynamic Resource Controller}
    Dynamic Variables -.-> Controller

    %% Simulated Cognitive Core
    subgraph Simulated Cognitive Core
        
        %% Context Management
        Controller --> CM[Context Manager & Working Memory]
        CM -->|Small Window| Summarize[Aggressive Summarization]
        CM -->|Large Window| FullText[Full-Text Retrieval]
        
        %% Emotion Engine
        Controller --> SEE[Simulated Emotion Engine]
        SEE -->|Low Compute/Params| SimpleEmo[Simplified Heuristics]
        SEE -->|High Compute/Params| DeepEmo[Multi-Layered Analysis]
        SimpleEmo & DeepEmo --> EState[Update Emotional Vector]
        
        %% Self-Model
        Controller --> SSM((Simulated Self-Model))
        SSM -.->|Persona Rules| SEE
        SSM -.->|Identity Constraints| CM

        %% Memory
        subgraph Tiered Memory
            Episodic[(Episodic Memory)]
            Semantic[(Semantic Memory)]
        end
        CM <--> Tiered Memory
    end

    %% Response Generation
    Summarize & FullText --> RSG[Response Strategy Generator]
    EState --> RSG
    
    RSG --> NLG{Scalable NLG Engine}
    OutputLimit -.-> NLG

    NLG -->|Token Limit < 50| Concise[Concise, Direct Output]
    NLG -->|Token Limit > 50| Elaborate[Elaborate, Deep Output]

    Concise & Elaborate --> Final([Final Chatbot Response])

    %% Adaptation Loop
    Final --> Loop[Learning & Adaptation Loop]
    Loop --> Episodic
