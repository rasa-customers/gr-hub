# Evergreen
An evergreen template to build demos off of.

```diff
@@ Cheatsheet @@

+ Train
rasa train
rasa train -c config.yml -d domain

+ Inspect
rasa inspect --debug
rasa inspect --debug --log-file logs.txt

+ Run
rasa run
rasa run actions --debug --auto-reload
rasa run --debug --log-file logs.txt --enable-api --cors "*"
rasa run --debug --log-file $(LOG_DIR)/logs_$(shell date +%Y%m%d%H%M%S).out --enable-api --cors "*"

+ Shell
rasa shell --debug

+ E2e
rasa test e2e tests/


- Deprecated
rasa studio config --advanced
rasa studio login
rasa studio download RASA_STUDIO_ASSISTANT_NAME -d domain
rasa studio upload -c config.yml -d domain --calm
```

## NLU

```diff
! config.yml
pipeline:
  - name: WhitespaceTokenizer
  - name: RegexFeaturizer
  - name: LexicalSyntacticFeaturizer
  - name: CountVectorsFeaturizer
  - name: CountVectorsFeaturizer
    analyzer: char_wb
    min_ngram: 1
    max_ngram: 4
  - name: LogisticRegressionClassifier
  - name: NLUCommandAdapter

! flows.yml
flow_get_store_hours:
  ...
  nlu_trigger:
    - intent: store_hours
      confidence_threshold: 0.9
  ...
```

## Prompts

```
Conversation turns included in the current context window.
{{ current_conversation }}

The user’s latest message.
{{ user_message }}

Full tracker object (structured state).
{{ tracker }}
  {{ tracker.slots }}
  {{ tracker.latest_message }}
  {{ tracker.active_flow }}
	{{ active_flow }}

Always available
{{ available_flows }}
{{ current_conversation }}
{{ flow_slots }}
{{ current_flow }}
{{ current_slot }}
{{ current_slot_description }}
{{ current_slot_type }}
{{ current_slot_allowed_values }}

Current step metadata 
{{ step }}

Slots
* Call by name
{{ slot_user_wants_7mo_cd }}
{{ slots.slot_name }}
{{ tracker.slots.slot_name }}

Session metadata:
{{ session_started_metadata }}

In command generation prompts, Rasa injects structured data such as:
{{ flows }}
{{ available_flows }}
{{ flow_descriptions }}
{{ available_actions }}

RAG / Enterprise Search
* If using EnterpriseSearchPolicy or SearchReadyLLMCommandGenerator:
{{ retrieved_documents }}
{{ documents }}
{{ context_documents }}
{{ search_results }}

Rephraser
{{ suggested_response }}
{{ draft_response }}
{{ response }}

Time
{{ current_date }}
{{ current_time }}
{{ day_of_week }}

Sub Agents?
{{ active_agent }}
{{ completed_agents }}











