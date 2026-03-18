# Action Operations (245)

> Executable steps that perform work within a workflow.


## 1. Add Comment

- **Slug:** `add_comment`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.add_comment`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval

Adds a comment to an object.

### Input (`input`)

- `object` (id) **(required)** — ID of the object
  - ID types: `account`, `capability`, `conversation`, `enhancement`, `feature`, `incident`, `issue`, `linkable`, `opportunity`, `product`, `meeting`, `revo`, `revu`, `runnable`, `ticket`, `comment`, `dm`, `article`
- `body` (text) **(required)** — Comment body
- `visibility` (enum) — Specifies the entry's visibility. 'Internal' limits visibility to the Dev organization, 'External' allows visibility to both the Dev organization and its customers, and 'Private' limits visibility to specified users only. Defaults to 'External' if unspecified.
  - Allowed: `external`, `internal`, `private`
  - Default: `external`

### Output (`output`)

- `id` (id) **(required)** — ID of the comment
  - ID types: `comment`
- `created_by` (composite) **(required)** — ID of the user who created the comment
  - `type` (enum) **(required)** — Type of the user
    - Allowed: `dev_user`, `sys_user`, `rev_user`, `service_account`
  - `id` (id) **(required)** — ID of the user
    - ID types: `devu`, `sysu`, `revu`, `svcacc`
  - `display_id` (text) **(required)** — Display ID of the user
- `created_by_agent` (composite) — ID of the service account who created the comment on behalf of the user
  - `id` (id) — ID of the service account
    - ID types: `svcacc`
  - `display_id` (text) **(required)** — Display ID of the agent

---

## 2. Agent Callback

- **Slug:** `agent_callback`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.agent_callback`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `ai_agent:execute`

Takes the Output of the workflow and returns it to the agent

### Input (`input`)

- `agent_session_id` (id) **(required)** — The ID of the agent to send the output to
  - ID types: `ai_agent_session`
- `skill_call_id` (text) **(required)** — The ID of the skill call corresponding to the workflow
- `skill_name` (text) **(required)** — The name of the skill call corresponding to the workflow
- `output` (struct) **(required)** — The output to return to the agent

---

## 3. AI Feedback

- **Slug:** `ai_feedback`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.ai_feedback`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Accepts and logs the feedback on the AI operation done in this workflow to gain insights and improve accuracy of the AI operation

### Input (`input`)

- `session_id` (tokens) **(required)** — Session ID returned by AI operation
- `request_id` (tokens) — Request ID returned by AI operation
- `feedback_type` (enum) **(required)** — Type of the feedback
  - Allowed: `binary`
  - Default: `binary`
- `binary_feedback` (bool) **(required)** — Do you agree with the AI operation's output?
- `feedback_reason` (text) — Reason for the given feedback

---

## 4. Ask Agent

- **Slug:** `ask_agent`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.ask_agent`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `ai_agent:read`

Ask an agent for information

---

## 5. Ask AI

- **Slug:** `ask_ai`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.ask_ai`
- **Tags:** HIPAA, Agent Skill, Workflow Node

AI-powered action node for transforming, analyzing, and generating text content. Use Normal mode for everyday tasks like classification, simple decisions. Use Reasoning mode for advanced analysis.

### Input (`input`)

- `ai_input` (text) **(required)** — The input prompt based on which the AI response is generated

### Output (`output`)

- `ai_output` (text) — Output string from the AI Action node

---

## 6. Ask Options

- **Slug:** `ask_options`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.ask_options`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Send buttons and content to a user

---

## 7. Classify Object

- **Slug:** `classify_object`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.classify_object`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Classifies an object into a set of customisable categories

### Input (`input`)

- `object_id` (id) **(required)** — ID of the object
  - ID types: `issue`, `ticket`, `conversation`, `enhancement`
- `object_type` (enum) **(required)** — Type of the object
  - Allowed: `issue`, `ticket`, `conversation`, `enhancement`
  - Default: `issue`
- `categories` (array) **(required)** — Specify the categories into which the object should be classified
  - `name` (tokens) **(required)** — Name of the category into which the object can be classified
  - `description` (text) — Describe which type of objects should be categorized into this category
- `guidelines` (text) — Specify any special instructions or examples that should be used by the classifier

### Output (`output`)

- `category` (text) — Output category from the object classification action node
- `justification` (text) — Output category justification from the object classification action node
- `session_id` (tokens) **(required)** — Session ID generated for this AI operation

---

## 8. Convert Conversation To Ticket

- **Slug:** `convert_conversation_to_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.convert_conversation_to_ticket`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `conversation:read`, `link:read`

Converts a conversation to a ticket. Only Conversations from PLuG and Slack can be converted to tickets.

### Input (`input`)

- `conversation_id` (id) **(required)** — ID of the conversation to be converted to a ticket.
  - ID types: `conversation`

### Output (`output`)

- `ticket_id` (id) **(required)** — ID of the ticket created from the conversation.
  - ID types: `ticket`

---

## 9. Create AAI Project

- **Slug:** `create_aai_proj`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_aai_proj`

Creates AAI Project

---

## 10. Create Abc

- **Slug:** `create_abc`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_abc`
- **Tags:** HIPAA, ALPHA

Creates Abc

---

## 11. Create Account

- **Slug:** `create_account`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_account`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `account:write`

Creates an account

---

## 12. Create Ai Agent Session Metrics

- **Slug:** `create_ai_agent_session_metrics`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_ai_agent_session_metrics`

Creates Ai Agent Session Metrics

---

## 13. Create Alert

- **Slug:** `create_alert`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_alert`

Creates Alert

---

## 14. Create Article

- **Slug:** `create_article`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_article`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `article:write`

Creates an article

---

## 15. Create Pending Record

- **Slug:** `create_as_pend`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_as_pend`

Creates Pending Record

---

## 16. Create Asagentlog

- **Slug:** `create_asagentlog`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_asagentlog`

Creates Asagentlog

---

## 17. Create Asownerlog

- **Slug:** `create_asownerlog`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_asownerlog`
- **Tags:** HIPAA, ALPHA

Creates Asownerlog

---

## 18. Create Asreport

- **Slug:** `create_asreport`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_asreport`

Creates Asreport

---

## 19. Create Asset

- **Slug:** `create_asset`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_asset`

Creates Asset

---

## 20. Create Campaigned

- **Slug:** `create_campaigned`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_campaigned`

Creates Campaigned

---

## 21. Create Chameleon Survey

- **Slug:** `create_chameleon_survey`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_chameleon_survey`

Creates Chameleon Survey

---

## 22. Create Change

- **Slug:** `create_change`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_change`

Creates Change

---

## 23. Create Chat

- **Slug:** `create_chat`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_chat`

Creates Chat

---

## 24. Create Code Change

- **Slug:** `create_code_change`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_code_change`
- **Tags:** HIPAA

Creates Code Change

---

## 25. Create Contact

- **Slug:** `create_contact`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_contact`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `rev_user:read`

Creates a contact

---

## 26. Create Cost

- **Slug:** `create_cost`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_cost`

Creates Cost

---

## 27. Create Devrev  Api

- **Slug:** `create_devrev__api`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_devrev__api`

Creates Devrev  Api

---

## 28. Create DM

- **Slug:** `create_dm`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_dm`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `dm:write`

Creates message to initiate a private conversation between the given users

---

## 29. Create Articulate(reach360-o):Courses-fea0ea393c

- **Slug:** `create_drad_cour`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_drad_cour`

Creates Articulate(reach360-o):Courses-fea0ea393c

---

## 30. Create Google_Cal(devrev.ai):Invitee-8fc70d7ea5

- **Slug:** `create_drad_invi`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_drad_invi`

Creates Google_Cal(devrev.ai):Invitee-8fc70d7ea5

---

## 31. Create Articulate(reach360-o):Learners-63b4ae28b4

- **Slug:** `create_drad_lear`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_drad_lear`

Creates Articulate(reach360-o):Learners-63b4ae28b4

---

## 32. Create EastWest

- **Slug:** `create_eastwest`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_eastwest`

Creates EastWest

---

## 33. Create Enhancement

- **Slug:** `create_enhancement`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_enhancement`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `enhancement:write`

Creates an enhancement

---

## 34. Create Entity

- **Slug:** `create_entity`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_entity`

Creates Entity

---

## 35. Create Event

- **Slug:** `create_event`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_event`

Creates Event

---

## 36. Create Expense

- **Slug:** `create_expense`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_expense`

Creates Expense

---

## 37. Create Form

- **Slug:** `create_form`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_form`

Creates Form

---

## 38. Create Implementation

- **Slug:** `create_implementation`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_implementation`

Creates Implementation

---

## 39. Create Incident

- **Slug:** `create_incident`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_incident`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `incident:write`

Creates an incident

---

## 40. Create Issue

- **Slug:** `create_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_issue`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `issue:write`

Creates an issue

---

## 41. Create Log Pattern

- **Slug:** `create_log_pattern`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_log_pattern`

Creates Log Pattern

---

## 42. Create Meeting

- **Slug:** `create_meeting`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_meeting`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `meeting:write`

Creates a meeting

---

## 43. Create Mypod

- **Slug:** `create_mypod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_mypod`
- **Tags:** HIPAA, ALPHA

Creates Mypod

---

## 44. Create On Call Group

- **Slug:** `create_on_call_group`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_on_call_group`

Creates On Call Group

---

## 45. Create On Call Schedule

- **Slug:** `create_on_call_schedule`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_on_call_schedule`

Creates On Call Schedule

---

## 46. Create Oncall

- **Slug:** `create_oncall`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_oncall`

Creates Oncall

---

## 47. Create Oncallsup

- **Slug:** `create_oncallsup`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_oncallsup`

Creates Oncallsup

---

## 48. Create Opportunity

- **Slug:** `create_opportunity`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_opportunity`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `opportunity:write`

Creates an opportunity

---

## 49. Create Part Recommendation

- **Slug:** `create_part_recommendation`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_part_recommendation`

Creates Part Recommendation

---

## 50. Create Problem

- **Slug:** `create_problem`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_problem`

Creates Problem

---

## 51. Create Project

- **Slug:** `create_project`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_project`

Creates Project

---

## 52. Create Reco

- **Slug:** `create_reco`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_reco`

Creates Reco

---

## 53. Create Relationsh

- **Slug:** `create_relationsh`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_relationsh`

Creates Relationsh

---

## 54. Create Release

- **Slug:** `create_release`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_release`

Creates Release

---

## 55. Create Reviews

- **Slug:** `create_reviews`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_reviews`

Creates Reviews

---

## 56. Create Round Robin State

- **Slug:** `create_round_robin_state`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_round_robin_state`

Creates Round Robin State

---

## 57. Create Sales Pod

- **Slug:** `create_sales_pod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_sales_pod`
- **Tags:** HIPAA, ALPHA

Creates Sales Pod

---

## 58. Create SDR

- **Slug:** `create_sdr_alias`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_sdr_alias`
- **Tags:** HIPAA, ALPHA

Creates SDR

---

## 59. Create Sdr Pod

- **Slug:** `create_sdr_pod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_sdr_pod`
- **Tags:** HIPAA, ALPHA

Creates Sdr Pod

---

## 60. Create Se

- **Slug:** `create_se`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_se`
- **Tags:** HIPAA, ALPHA

Creates Se

---

## 61. Create Stream

- **Slug:** `create_stream`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_stream`

Creates Stream

---

## 62. Create Syncerrors

- **Slug:** `create_syncerrors`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_syncerrors`

Creates Syncerrors

---

## 63. Create Team

- **Slug:** `create_team`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_team`

Creates Team

---

## 64. Create Testresult

- **Slug:** `create_testresult`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_testresult`

Creates Testresult

---

## 65. Create Theme

- **Slug:** `create_theme`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_theme`

Creates Theme

---

## 66. Create Ticket

- **Slug:** `create_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.create_ticket`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `ticket:write`

Creates a ticket

---

## 67. Create Todo

- **Slug:** `create_todo`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_todo`

Creates Todo

---

## 68. Create Vendor

- **Slug:** `create_vendor`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_vendor`

Creates Vendor

---

## 69. Create Webinar

- **Slug:** `create_webinar`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.create_webinar`

Creates Webinar

---

## 70. Evaluate Sentiment

- **Slug:** `evaluate_sentiment`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.evaluate_sentiment`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Takes the ID of the object and evaluates the sentiment. Currently supports tickets and conversations.

### Input (`input`)

- `object` (id) **(required)** — The ID of the ticket or conversation to evaluate sentiment for
  - ID types: `ticket`, `conversation`

### Output (`output`)

- `sentiment` (enum) **(required)** — The evaluated sentiment. Takes one of the following values: delighted, frustrated, happy, neutral, unhappy, unknown
  - Allowed: `delighted`, `frustrated`, `happy`, `neutral`, `unhappy`, `unknown`
- `justification` (text) — The justification for the evaluated sentiment

---

## 71. Execute Analytics Job

- **Slug:** `execute_analytics_job`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.execute_analytics_job`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `serengeti_job:execute`

Executes analytics job. Uses the analytics jobs execute API endpoint.

### Input (`input`)

- `id` (id) **(required)** — Job ID of the analytics job to be executed
  - ID types: `oasis_job`
- `scheduled_at` (timestamp) — The scheduled time when the analytics job should be executed

---

## 72. Execute Metric Action

- **Slug:** `execute_metric_action`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.execute_metric_action`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Handles the execution of metric actions (such as Start, Complete, Restart, Pause, Resume) on a particular SLA metric tracked on a given object.

---

## 73. Extract Content

- **Slug:** `extract_content`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.extract_content`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `artifact:read`

Extracts the content from a resource artifact or URL.

### Input (`input`)

- `input_format` (uenum) **(required)** — The format of the input to extract content from.
  - Default: `1`
- `max_content_length` (int) — The maximum number of content to return in KB. Default is 500KB.

### Output (`output`)

- `artifact_id` (id) **(required)** — The ID of the artifact containing the extracted content.
  - ID types: `artifact`
- `is_truncated` (bool) — Whether the content was truncated.

---

## 74. Fetch Object Context

- **Slug:** `fetch_object_context`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.fetch_object_context`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `link:read`, `artifact:read`

Gets contextual information about any DevRev object

### Input (`input`)

- `object_id` (text) **(required)** — The ID or display-id of the DevRev object.

### Output (`output`)

- `object_context` (text) — The context of the DevRev object.

---

## 75. Get AAI Project

- **Slug:** `get_aai_proj`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_aai_proj`

Gets the AAI Project record details

---

## 76. Get Abc

- **Slug:** `get_abc`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_abc`
- **Tags:** ALPHA

Gets the Abc record details

---

## 77. Get Account

- **Slug:** `get_account`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_account`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `account:read`

Gets the account details for the given account ID

---

## 78. Get Ai Agent Session Metrics

- **Slug:** `get_ai_agent_session_metrics`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_ai_agent_session_metrics`

Gets the Ai Agent Session Metrics record details

---

## 79. Get AirSync Sync Unit

- **Slug:** `get_airdrop_sync_unit`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_airdrop_sync_unit`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `sync_unit:read`

Fetches the details of the AirSync Sync Unit with the given ID

---

## 80. Get Alert

- **Slug:** `get_alert`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_alert`

Gets the Alert record details

---

## 81. Get Pending Record

- **Slug:** `get_as_pend`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_as_pend`

Gets the Pending Record record details

---

## 82. Get Asagentlog

- **Slug:** `get_asagentlog`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_asagentlog`

Gets the Asagentlog record details

---

## 83. Get Asownerlog

- **Slug:** `get_asownerlog`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_asownerlog`
- **Tags:** ALPHA

Gets the Asownerlog record details

---

## 84. Get Asreport

- **Slug:** `get_asreport`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_asreport`

Gets the Asreport record details

---

## 85. Get Asset

- **Slug:** `get_asset`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_asset`

Gets the Asset record details

---

## 86. Get Campaigned

- **Slug:** `get_campaigned`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_campaigned`

Gets the Campaigned record details

---

## 87. Get Chameleon Survey

- **Slug:** `get_chameleon_survey`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_chameleon_survey`

Gets the Chameleon Survey record details

---

## 88. Get Change

- **Slug:** `get_change`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_change`

Gets the Change record details

---

## 89. Get Chat

- **Slug:** `get_chat`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_chat`

Gets the Chat record details

---

## 90. Get Code Change

- **Slug:** `get_code_change`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_code_change`

Gets the Code Change record details

---

## 91. Get Conversation

- **Slug:** `get_conversation`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_conversation`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `conversation:read`

Gets the conversation details for the given ID

---

## 92. Get Cost

- **Slug:** `get_cost`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_cost`

Gets the Cost record details

---

## 93. Get Customer

- **Slug:** `get_customer`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_customer`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `rev_user:read`

Fetch details of the contact user with the given ID

---

## 94. Get Devrev  Api

- **Slug:** `get_devrev__api`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_devrev__api`

Gets the Devrev  Api record details

---

## 95. Get Articulate(reach360-o):Courses-fea0ea393c

- **Slug:** `get_drad_cour`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_drad_cour`

Gets the Articulate(reach360-o):Courses-fea0ea393c record details

---

## 96. Get Google_Cal(devrev.ai):Invitee-8fc70d7ea5

- **Slug:** `get_drad_invi`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_drad_invi`

Gets the Google_Cal(devrev.ai):Invitee-8fc70d7ea5 record details

---

## 97. Get Articulate(reach360-o):Learners-63b4ae28b4

- **Slug:** `get_drad_lear`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_drad_lear`

Gets the Articulate(reach360-o):Learners-63b4ae28b4 record details

---

## 98. Get EastWest

- **Slug:** `get_eastwest`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_eastwest`

Gets the EastWest record details

---

## 99. Get Enhancement

- **Slug:** `get_enhancement`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_enhancement`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `enhancement:read`

Gets an enhancement by ID

---

## 100. Get Entity

- **Slug:** `get_entity`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_entity`

Gets the Entity record details

---

## 101. Get Event

- **Slug:** `get_event`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_event`

Gets the Event record details

---

## 102. Get Expense

- **Slug:** `get_expense`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_expense`

Gets the Expense record details

---

## 103. Get Feature

- **Slug:** `get_feature`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_feature`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `feature:read`

Gets the feature details for the given ID

---

## 104. Get Form

- **Slug:** `get_form`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_form`

Gets the Form record details

---

## 105. Get Implementation

- **Slug:** `get_implementation`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_implementation`

Gets the Implementation record details

---

## 106. Get Incident

- **Slug:** `get_incident`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_incident`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `incident:read`

Fetches details of the incident with the given ID

---

## 107. Get Issue

- **Slug:** `get_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_issue`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `issue:read`

Gets an issue by ID

---

## 108. Get Log Pattern

- **Slug:** `get_log_pattern`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_log_pattern`

Gets the Log Pattern record details

---

## 109. Get Meeting

- **Slug:** `get_meeting`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_meeting`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `meeting:read`

Gets the meeting details for the given meeting ID

---

## 110. Get Metric Trackers

- **Slug:** `get_metric_trackers`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_metric_trackers`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Fetches the SLA metric trackers for a given object.

---

## 111. Get Mypod

- **Slug:** `get_mypod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_mypod`
- **Tags:** ALPHA

Gets the Mypod record details

---

## 112. Get On Call Group

- **Slug:** `get_on_call_group`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_on_call_group`

Gets the On Call Group record details

---

## 113. Get On Call Schedule

- **Slug:** `get_on_call_schedule`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_on_call_schedule`

Gets the On Call Schedule record details

---

## 114. Get Oncall

- **Slug:** `get_oncall`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_oncall`

Gets the Oncall record details

---

## 115. Get Oncallsup

- **Slug:** `get_oncallsup`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_oncallsup`

Gets the Oncallsup record details

---

## 116. Get Opportunity

- **Slug:** `get_opportunity`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_opportunity`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `opportunity:read`

Gets the opportunity details for the given opportunity ID

---

## 117. Get Organization User

- **Slug:** `get_org_user`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_org_user`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `dev_user:read`

Retrieves details of a specific user in the organization

---

## 118. Organization User Preference

- **Slug:** `get_org_user_preference`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_org_user_preference`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `preference:read`

Fetch preference of the organization user with the given user ID

---

## 119. Get Part

- **Slug:** `get_part`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_part`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node

Gets a part by ID

---

## 120. Get Part Recommendation

- **Slug:** `get_part_recommendation`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_part_recommendation`

Gets the Part Recommendation record details

---

## 121. Get Problem

- **Slug:** `get_problem`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_problem`

Gets the Problem record details

---

## 122. Get Project

- **Slug:** `get_project`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_project`

Gets the Project record details

---

## 123. Get Reco

- **Slug:** `get_reco`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_reco`

Gets the Reco record details

---

## 124. Get Relationsh

- **Slug:** `get_relationsh`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_relationsh`

Gets the Relationsh record details

---

## 125. Get Release

- **Slug:** `get_release`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_release`

Gets the Release record details

---

## 126. Get Reviews

- **Slug:** `get_reviews`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_reviews`

Gets the Reviews record details

---

## 127. Get Round Robin State

- **Slug:** `get_round_robin_state`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_round_robin_state`

Gets the Round Robin State record details

---

## 128. Get Sales Pod

- **Slug:** `get_sales_pod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_sales_pod`
- **Tags:** ALPHA

Gets the Sales Pod record details

---

## 129. Get SDR

- **Slug:** `get_sdr_alias`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_sdr_alias`
- **Tags:** ALPHA

Gets the SDR record details

---

## 130. Get Sdr Pod

- **Slug:** `get_sdr_pod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_sdr_pod`
- **Tags:** ALPHA

Gets the Sdr Pod record details

---

## 131. Get Se

- **Slug:** `get_se`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_se`
- **Tags:** ALPHA

Gets the Se record details

---

## 132. Get Stream

- **Slug:** `get_stream`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_stream`

Gets the Stream record details

---

## 133. Get Syncerrors

- **Slug:** `get_syncerrors`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_syncerrors`

Gets the Syncerrors record details

---

## 134. Get Team

- **Slug:** `get_team`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_team`

Gets the Team record details

---

## 135. Get Testresult

- **Slug:** `get_testresult`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_testresult`

Gets the Testresult record details

---

## 136. Get Theme

- **Slug:** `get_theme`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_theme`

Gets the Theme record details

---

## 137. Get Ticket

- **Slug:** `get_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_ticket`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `ticket:read`

Gets a ticket by ID

---

## 138. Get Time

- **Slug:** `get_time`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_time`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Perform time-related actions

### Input (`input`)

- `action` (enum) **(required)** — The action to perform
  - Allowed: `get_current_time`
  - Default: `get_current_time`
- `time_zone` (enum) **(required)** — The time zone offset to use for the time
  - Allowed: `-12:00`, `-11:00`, `-11:30`, `-10:00`, `-10:30`, `-09:00`, `-09:30`, `-08:00`, `-08:30`, `-07:00`, `-07:30`, `-06:00`, `-06:30`, `-05:00`, `-05:30`, `-04:00`, `-04:30`, `-03:00`, `-03:30`, `-02:00`, `-02:30`, `-01:00`, `-01:30`, `+00:00`, `-00:30`, `+00:30`, `+01:00`, `+01:30`, `+02:00`, `+02:30`, `+03:00`, `+03:30`, `+04:00`, `+04:30`, `+05:00`, `+05:30`, `+05:45`, `+06:00`, `+06:30`, `+07:00`, `+07:30`, `+08:00`, `+08:30`, `+08:45`, `+09:00`, `+09:30`, `+10:00`, `+10:30`, `+11:00`, `+11:30`, `+12:00`, `+12:45`, `+13:00`, `+14:00`
  - Default: `+00:00`

### Output (`output`)

- `unix_seconds` (int) **(required)** — The time in unix seconds, i.e., the number of seconds since 1970-01-01 00:00:00 UTC.
- `timestamp` (timestamp) **(required)** — The time as a string of format: 2024-05-22T12:00:00.000Z. Compatible with DevRev APIs.
- `day` (enum) **(required)** — The day of the week.
  - Allowed: `Sunday`, `Monday`, `Tuesday`, `Wednesday`, `Thursday`, `Friday`, `Saturday`
- `hour` (int) **(required)** — The hour of the day.
- `minute` (int) **(required)** — The minute of the hour.
- `date` (int) **(required)** — The date of the month.
- `month` (int) **(required)** — The month of the year.
- `year` (int) **(required)** — The year.

---

## 139. Get Todo

- **Slug:** `get_todo`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_todo`

Gets the Todo record details

---

## 140. Get Vendor

- **Slug:** `get_vendor`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_vendor`

Gets the Vendor record details

---

## 141. Get Webinar

- **Slug:** `get_webinar`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.get_webinar`

Gets the Webinar record details

---

## 142. Get Workspace

- **Slug:** `get_workspace`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.get_workspace`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `rev_org:read`

Fetch details of the workspace with the given ID

---

## 143. HTTP

- **Slug:** `http`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.http`
- **Tags:** Agent Skill, Workflow Node

Make HTTP requests

### Input (`input`)

- `method` (enum) — Method of the HTTP request
  - Allowed: `GET`, `POST`, `PUT`, `DELETE`
  - Default: `GET`
- `query_params` (array) — Query parameters of the HTTP request
  - `key` (text) **(required)** — Key
  - `value` (text) — Value
- `auth_type` (enum) — Authentication type
  - Allowed: `Bearer`, `Basic`, `None`
  - Default: `None`
- `url` (text) **(required)** — URL of the HTTP request
- `headers` (array) — Headers of the HTTP request
  - `key` (text) **(required)** — Key
  - `value` (text) — Value
- `body` (text) — Body of the HTTP request

### Output (`output`)

- `status_code` (int) — Status code of the HTTP request
- `body` (text) — Body of the HTTP request
- `headers` (array) — Headers of the HTTP request
  - `key` (text) **(required)** — Key
  - `value` (text) — Value
- `content_type` (text) — Content type of the HTTP response

---

## 144. Hybrid Search

- **Slug:** `hybrid_search`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.hybrid_search`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node

Intelligent search across DevRev's knowledge graph to find and discover objects

### Input (`input`)

- `query` (text) **(required)** — Search query
- `namespace` (uenum) **(required)** — Namespace to search in
- `limit` (int) — Maximum number of results to return. Default is 10.

---

## 145. Execute Code

- **Slug:** `invoke_code`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.invoke_code`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Executes code as an operation.

---

## 146. Knowledge Store Deindex

- **Slug:** `knowledge_store_deindex`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.knowledge_store_deindex`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `knowledge_node:write`

Remove indexing for a given text or article.

### Input (`input`)

- `object` (id) **(required)** — The ID of the source object to deindex content for.
  - ID types: `account`, `capability`, `conversation`, `enhancement`, `feature`, `incident`, `issue`, `linkable`, `opportunity`, `product`, `revo`, `revu`, `runnable`, `ticket`, `meeting`, `widget`, `custom_object`, `article`, `workflow`, `operation`, `dataset`, `question_answer`, `dashboard`
- `content_descriptor` (text) **(required)** — Identifier that describes what the content is about. Eg: issue_summary, ticket_summary, comment_summary, etc.
- `use_case` (uenum) **(required)** — Use case identifier that defines the purpose of the indexed content.

---

## 147. Knowledge Store Index

- **Slug:** `knowledge_store_index`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.knowledge_store_index`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `knowledge_node:write`

Create or update indexing for a given text or article.

---

## 148. Link Conversation with Ticket

- **Slug:** `link_conversation_with_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.link_conversation_with_ticket`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `ticket:read`, `conversation:read`, `link:write`

Links a conversation with a ticket

### Input (`input`)

- `source` (id) **(required)** — ID of the conversation
  - ID types: `conversation`
- `target` (id) **(required)** — ID of the ticket
  - ID types: `ticket`

---

## 149. Link Incident with Issue

- **Slug:** `link_incident_with_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.link_incident_with_issue`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `incident:read`, `issue:read`, `link:write`

Links an incident with an issue

### Input (`input`)

- `source` (id) **(required)** — ID of the incident
  - ID types: `incident`
- `target` (id) **(required)** — ID of the issue
  - ID types: `issue`

---

## 150. Link Incident with Ticket

- **Slug:** `link_incident_with_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.link_incident_with_ticket`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `incident:read`, `ticket:read`, `link:write`

Links an incident with a ticket

### Input (`input`)

- `source` (id) **(required)** — ID of the incident
  - ID types: `incident`
- `target` (id) **(required)** — ID of the ticket
  - ID types: `ticket`

---

## 151. Link Issue with Issue

- **Slug:** `link_issue_with_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.link_issue_with_issue`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `issue:read`, `link:write`

Links an issue with another issue

### Input (`input`)

- `source` (id) **(required)** — ID of the source issue
  - ID types: `issue`
- `link_type` (enum) — Type of link used to define the relationship.
  - Allowed: `is_parent_of`, `is_dependent_on`, `is_duplicate_of`
- `target` (id) **(required)** — ID of the target issue
  - ID types: `issue`

---

## 152. Link Meeting with Ticket

- **Slug:** `link_meeting_with_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.link_meeting_with_ticket`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `meeting:read`, `ticket:read`, `link:write`

Links a meeting with a ticket

### Input (`input`)

- `source` (id) **(required)** — ID of the meeting
  - ID types: `meeting`
- `target` (id) **(required)** — ID of the ticket
  - ID types: `ticket`

---

## 153. Link Ticket with Issue

- **Slug:** `link_ticket_with_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.link_ticket_with_issue`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `ticket:read`, `issue:read`, `link:write`

Links a ticket with an issue

### Input (`input`)

- `source` (id) **(required)** — ID of the ticket
  - ID types: `ticket`
- `target` (id) **(required)** — ID of the issue
  - ID types: `issue`

---

## 154. List Enhancements

- **Slug:** `list_enhancements`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.list_enhancements`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `enhancement:read`

Lists enhancements

---

## 155. List Issues

- **Slug:** `list_issues`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.list_issues`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node
- **Scopes:** `issue:read`

Lists issues

---

## 156. List objects linked to account

- **Slug:** `list_objects_linked_to_account`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.list_objects_linked_to_account`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Gets the linked objects of an account

---

## 157. List objects linked to issue

- **Slug:** `list_objects_linked_to_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.list_objects_linked_to_issue`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `issue:read`, `link:read`

Gets the linked objects of an issue

---

## 158. List objects linked to meeting

- **Slug:** `list_objects_linked_to_meeting`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.list_objects_linked_to_meeting`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Gets the linked objects of a meeting

---

## 159. List objects linked to opportunity

- **Slug:** `list_objects_linked_to_opportunity`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.list_objects_linked_to_opportunity`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Gets the linked objects of an opportunity

---

## 160. List objects linked to ticket

- **Slug:** `list_objects_linked_to_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.list_objects_linked_to_ticket`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `ticket:read`, `link:read`

Gets the linked objects of a ticket

---

## 161. Loop Over Articles

- **Slug:** `loop_over_articles`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_articles`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists articles based on the given filters and performs operations on them.

---

## 162. Loop Over Customers

- **Slug:** `loop_over_customers`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_customers`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists customers based on the given filters and performs operations on them.

---

## 163. Loop Over Users

- **Slug:** `loop_over_dev_users`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_dev_users`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists dev users based on the given filters and performs operations on them.

---

## 164. Loop Over Enhancements

- **Slug:** `loop_over_enhancements`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_enhancements`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `enhancement:read`

Lists enhancements based on the given filters and performs operations on them.

---

## 165. Loop Over Incidents

- **Slug:** `loop_over_incidents`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_incidents`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists incidents based on the given filters and performs operations on them.

---

## 166. Loop Over Issues

- **Slug:** `loop_over_issues`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_issues`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `issue:read`

Lists issues based on the given filters and performs operations on them.

---

## 167. Loop Over Meetings

- **Slug:** `loop_over_meetings`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_meetings`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists meetings based on the given filters and performs operations on them.

---

## 168. Loop Over Objects Linked to Account

- **Slug:** `loop_over_objects_linked_to_account`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_objects_linked_to_account`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists objects linked to an account and performs operations on them.

---

## 169. Loop Over Objects Linked to Enhancement

- **Slug:** `loop_over_objects_linked_to_enhancement`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_objects_linked_to_enhancement`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists objects linked to an enhancement and performs operations on them.

---

## 170. Loop Over Objects Linked to Issue

- **Slug:** `loop_over_objects_linked_to_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_objects_linked_to_issue`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists objects linked to an issue and performs operations on them.

---

## 171. Loop Over Objects Linked to Opportunity

- **Slug:** `loop_over_objects_linked_to_opportunity`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_objects_linked_to_opportunity`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists objects linked to an opportunity and performs operations on them.

---

## 172. Loop Over Opportunity

- **Slug:** `loop_over_opportunity`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_opportunity`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `opportunity:read`

Lists opportunities based on the given filters and performs operations on them.

---

## 173. Loop Over Sprints

- **Slug:** `loop_over_sprints`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_sprints`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `sprint:read`

Lists sprints based on the given filters and performs operations on them.

---

## 174. Loop Over Tickets

- **Slug:** `loop_over_tickets`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.loop_over_tickets`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Lists tickets based on the given filters and performs operations on them.

---

## 175. Oasis SQL Execute

- **Slug:** `oasis_sql_execute`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.oasis_sql_execute`
- **Tags:** Agent Skill, Workflow Node
- **Scopes:** `oasis_data:read`

Executes a SQL query on an Oasis dataset

### Input (`input`)

- `sql_query` (text) **(required)** — SQL query to execute

### Output (`output`)

- `data` (text) — Data returned by the SQL query

---

## 176. Spam Checker

- **Slug:** `object_spam_checker`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.object_spam_checker`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Checks if the given ticket or conversation is spam or not.

### Input (`input`)

- `id` (id) **(required)** — The ID of the ticket or conversation to check for spam
  - ID types: `ticket`, `conversation`

### Output (`output`)

- `is_spam` (bool) **(required)** — Returns true if object is spam, false otherwise

---

## 177. Pick User

- **Slug:** `pick_user`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.pick_user`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `object_member:read`

Picks an user from the group or list of users based on the given strategy.

---

## 178. Query Artifact Content

- **Slug:** `query_artifact_content`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.query_artifact_content`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `artifact:read`

Queries artifact and returns the relevant content. The provided token must have artifact creator permissions.

### Input (`input`)

- `artifact_id` (id) **(required)** — The ID of the artifact to query content from.
  - ID types: `artifact`
- `max_content_length` (int) — The maximum number of content to return in KB. Default is 500KB.

### Output (`output`)

- `response` (text) — The response from the query.
- `is_truncated` (bool) — Whether the content was truncated.

---

## 179. Send Notification

- **Slug:** `send_notification`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.send_notification`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `notification_content_template:all`, `notification:write`

Send notification to users or groups. By default, notification priority will be marked as "Important". Emails are sent for unread "Important" notifications. Once the workflow is deployed, any receiver can go notification preferences and change the priority to "Others" to stop receiving emails.

### Input (`input`)

- `receivers` (array) **(required)** — The users or groups who will receive the notification
- `body` (rich_text) **(required)** — The body of the notification
- `title` (rich_text) **(required)** — The title of the notification
- `content_template_id` (id) — The content template for the notification
  - ID types: `notification_content_template`
- `linked_object` (id) — The object which will open on clicking the notification
  - ID types: `account`, `capability`, `enhancement`, `feature`, `incident`, `issue`, `linkable`, `opportunity`, `product`, `runnable`, `ticket`, `conversation`

---

## 180. Set Agent Skill Output

- **Slug:** `set_ai_agent_skill_output`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.set_ai_agent_skill_output`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Sets the output of the AI agent skill

### Input (`input`)

- `outputs` (array) **(required)** — Outputs of the skill. Specify the key and select the value for each key.
  - `key` (text) **(required)** — The key for the output. For e.g. 'output', 'api_response', etc.
  - `value` (json_value) — The value for the key
    - Default: `None`

---

## 181. Suggest Part

- **Slug:** `suggest_part`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.suggest_part`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Suggest part that can be attributed to a given object

### Input (`input`)

- `id` (id) **(required)** — The ID of the ticket, conversation or incident to suggest parts for
  - ID types: `ticket`, `conversation`, `incident`

### Output (`output`)

- `id` (id) **(required)** — The ID of the suggested part
  - ID types: `capability`, `enhancement`, `feature`, `product`, `linkable`, `runnable`
- `found` (bool) **(required)** — If suggestion is present, returns true

---

## 182. Talk to Agent

- **Slug:** `talk_to_agent`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.talk_to_agent`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `ai_agent:read`

Talk to an agent on the specified object

---

## 183. Update AAI Project

- **Slug:** `update_aai_proj`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_aai_proj`

Updates the AAI Project

---

## 184. Update Abc

- **Slug:** `update_abc`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_abc`
- **Tags:** ALPHA

Updates the Abc

---

## 185. Update Account

- **Slug:** `update_account`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_account`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `account:write`

Updates an account

---

## 186. Update Ai Agent Session Metrics

- **Slug:** `update_ai_agent_session_metrics`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_ai_agent_session_metrics`

Updates the Ai Agent Session Metrics

---

## 187. Update Alert

- **Slug:** `update_alert`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_alert`

Updates the Alert

---

## 188. Update Article

- **Slug:** `update_article`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_article`
- **Tags:** HIPAA, Agent Skill, Workflow Node

Updates an article

---

## 189. Update Pending Record

- **Slug:** `update_as_pend`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_as_pend`

Updates the Pending Record

---

## 190. Update Asagentlog

- **Slug:** `update_asagentlog`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_asagentlog`

Updates the Asagentlog

---

## 191. Update Asownerlog

- **Slug:** `update_asownerlog`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_asownerlog`
- **Tags:** ALPHA

Updates the Asownerlog

---

## 192. Update Asreport

- **Slug:** `update_asreport`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_asreport`

Updates the Asreport

---

## 193. Update Asset

- **Slug:** `update_asset`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_asset`

Updates the Asset

---

## 194. Update Campaigned

- **Slug:** `update_campaigned`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_campaigned`

Updates the Campaigned

---

## 195. Update Chameleon Survey

- **Slug:** `update_chameleon_survey`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_chameleon_survey`

Updates the Chameleon Survey

---

## 196. Update Change

- **Slug:** `update_change`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_change`

Updates the Change

---

## 197. Update Chat

- **Slug:** `update_chat`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_chat`

Updates the Chat

---

## 198. Update Code Change

- **Slug:** `update_code_change`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_code_change`

Updates the Code Change

---

## 199. Update Contact

- **Slug:** `update_contact`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_contact`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `rev_user:write`

Updates a contact

---

## 200. Update Conversation

- **Slug:** `update_conversation`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_conversation`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `conversation:write`

Update a conversation

---

## 201. Update Cost

- **Slug:** `update_cost`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_cost`

Updates the Cost

---

## 202. Update Devrev  Api

- **Slug:** `update_devrev__api`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_devrev__api`

Updates the Devrev  Api

---

## 203. Update Articulate(reach360-o):Courses-fea0ea393c

- **Slug:** `update_drad_cour`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_drad_cour`

Updates the Articulate(reach360-o):Courses-fea0ea393c

---

## 204. Update Google_Cal(devrev.ai):Invitee-8fc70d7ea5

- **Slug:** `update_drad_invi`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_drad_invi`

Updates the Google_Cal(devrev.ai):Invitee-8fc70d7ea5

---

## 205. Update Articulate(reach360-o):Learners-63b4ae28b4

- **Slug:** `update_drad_lear`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_drad_lear`

Updates the Articulate(reach360-o):Learners-63b4ae28b4

---

## 206. Update EastWest

- **Slug:** `update_eastwest`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_eastwest`

Updates the EastWest

---

## 207. Update Enhancement

- **Slug:** `update_enhancement`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_enhancement`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `enhancement:write`

Updates an existing enhancement

---

## 208. Update Entity

- **Slug:** `update_entity`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_entity`

Updates the Entity

---

## 209. Update Event

- **Slug:** `update_event`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_event`

Updates the Event

---

## 210. Update Expense

- **Slug:** `update_expense`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_expense`

Updates the Expense

---

## 211. Update Form

- **Slug:** `update_form`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_form`

Updates the Form

---

## 212. Update Implementation

- **Slug:** `update_implementation`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_implementation`

Updates the Implementation

---

## 213. Update Incident

- **Slug:** `update_incident`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_incident`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `incident:write`

Updates an incident

---

## 214. Update Issue

- **Slug:** `update_issue`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_issue`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `issue:write`

Updates an issue

---

## 215. Update Log Pattern

- **Slug:** `update_log_pattern`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_log_pattern`

Updates the Log Pattern

---

## 216. Update Meeting

- **Slug:** `update_meeting`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_meeting`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `meeting:write`

Updates a meeting

---

## 217. Update Mypod

- **Slug:** `update_mypod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_mypod`
- **Tags:** ALPHA

Updates the Mypod

---

## 218. Update On Call Group

- **Slug:** `update_on_call_group`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_on_call_group`

Updates the On Call Group

---

## 219. Update On Call Schedule

- **Slug:** `update_on_call_schedule`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_on_call_schedule`

Updates the On Call Schedule

---

## 220. Update Oncall

- **Slug:** `update_oncall`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_oncall`

Updates the Oncall

---

## 221. Update Oncallsup

- **Slug:** `update_oncallsup`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_oncallsup`

Updates the Oncallsup

---

## 222. Update Opportunity

- **Slug:** `update_opportunity`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_opportunity`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `opportunity:write`

Updates an opportunity

---

## 223. Update Part Recommendation

- **Slug:** `update_part_recommendation`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_part_recommendation`

Updates the Part Recommendation

---

## 224. Update Problem

- **Slug:** `update_problem`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_problem`

Updates the Problem

---

## 225. Update Project

- **Slug:** `update_project`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_project`

Updates the Project

---

## 226. Update Question Answer

- **Slug:** `update_question_answer`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_question_answer`
- **Tags:** HIPAA, Agent Skill, Workflow Node
- **Scopes:** `question_answer:write`

Updates Question Answer

---

## 227. Update Reco

- **Slug:** `update_reco`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_reco`

Updates the Reco

---

## 228. Update Relationsh

- **Slug:** `update_relationsh`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_relationsh`

Updates the Relationsh

---

## 229. Update Release

- **Slug:** `update_release`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_release`

Updates the Release

---

## 230. Update Reviews

- **Slug:** `update_reviews`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_reviews`

Updates the Reviews

---

## 231. Update Round Robin State

- **Slug:** `update_round_robin_state`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_round_robin_state`

Updates the Round Robin State

---

## 232. Update Sales Pod

- **Slug:** `update_sales_pod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_sales_pod`
- **Tags:** ALPHA

Updates the Sales Pod

---

## 233. Update SDR

- **Slug:** `update_sdr_alias`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_sdr_alias`
- **Tags:** ALPHA

Updates the SDR

---

## 234. Update Sdr Pod

- **Slug:** `update_sdr_pod`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_sdr_pod`
- **Tags:** ALPHA

Updates the Sdr Pod

---

## 235. Update Se

- **Slug:** `update_se`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_se`
- **Tags:** ALPHA

Updates the Se

---

## 236. Update Stream

- **Slug:** `update_stream`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_stream`

Updates the Stream

---

## 237. Update Syncerrors

- **Slug:** `update_syncerrors`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_syncerrors`

Updates the Syncerrors

---

## 238. Update Team

- **Slug:** `update_team`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_team`

Updates the Team

---

## 239. Update Testresult

- **Slug:** `update_testresult`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_testresult`

Updates the Testresult

---

## 240. Update Theme

- **Slug:** `update_theme`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_theme`

Updates the Theme

---

## 241. Update Ticket

- **Slug:** `update_ticket`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.update_ticket`
- **Tags:** HIPAA, MCP Tool, Agent Skill, Workflow Node, Needs Approval
- **Scopes:** `ticket:write`

Updates a ticket

---

## 242. Update Todo

- **Slug:** `update_todo`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_todo`

Updates the Todo

---

## 243. Update Vendor

- **Slug:** `update_vendor`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_vendor`

Updates the Vendor

---

## 244. Update Webinar

- **Slug:** `update_webinar`
- **Namespace:** `custom_object`
- **ID:** `operation-custom_object.update_webinar`

Updates the Webinar

---

## 245. Web Search

- **Slug:** `web_search`
- **Namespace:** `devrev`
- **ID:** `operation-devrev.web_search`
- **Tags:** Agent Skill, Workflow Node

Calls a web search API with a query and returns results. Use this tool to get latest information from the internet. Prerequisite: Must be used only if the information is not available inside devrev knowledge. Use ONLY as a last resort, and ONLY for genuinely external information. Web search is external and may not reflect your organization's context.

Always cite your answers using the URLs provided in the search results. Citations for URL can be shown using markdown : [title](URL)

eg: Tesla shares have lost 44% of their value since hitting an all-time high in December [Tesla sales plunge: Biggest decline in history | CNN Business](https://www.cnn.com/2025/04/02/business/tesla-sales/index.html)

### Input (`input`)

- `query` (text) **(required)** — The search query
- `max_results` (int) — Number of results to fetch

### Output (`output`)

- `results` (array) — Search results containing URLs and snippets
  - `url` (text) **(required)** — URL of the search result
  - `snippet` (text) — Snippet of the result content

---
