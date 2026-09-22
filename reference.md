# Reference
## events
<details><summary><code>client.events.<a href="/src/api/resources/events/client/Client.ts">queryEvents</a>({ ...params }) -> ChronicleLabsApi.EventListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. Results are scoped to the tenant of the API key and ordered newest first by event time and event ID. Pass the opaque `next_cursor` as `cursor` to continue without an offset scan.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.events.queryEvents();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.QueryEventsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EventsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.events.<a href="/src/api/resources/events/client/Client.ts">ingestEvent</a>({ ...params }) -> ChronicleLabsApi.IngestResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.events.ingestEvent({
    source: "my-agent",
    topic: "conversations",
    event_type: "message.sent"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.IngestRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EventsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.events.<a href="/src/api/resources/events/client/Client.ts">ingestEventBatch</a>({ ...params }) -> ChronicleLabsApi.IngestResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:write. Maximum 1000 events per batch; larger batches are rejected with 422. Request bodies over the size limit are rejected with 413. Each request consumes 10 rate-limit units.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.events.ingestEventBatch([{
        source: "my-agent",
        topic: "conversations",
        event_type: "message.sent"
    }]);

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.IngestRequest[]` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EventsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.events.<a href="/src/api/resources/events/client/Client.ts">streamEvents</a>({ ...params }) -> core.Stream&lt;ChronicleLabsApi.EventResult&gt;</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. A Server-Sent Events stream of events matching the optional filters, held open indefinitely.

Opening a stream consumes 5 rate-limit units.

Each message has `event: event` and a `data` field carrying one EventResult as JSON. A comment line arrives every 15 seconds so intermediaries do not close an idle connection.

Every message carries an opaque, stream-specific `id` backed by a monotonic per-tenant delivery sequence. It records ingestion order, independently of the source event's `event_time`. Record the last id you processed and do not parse or construct it.

When `Last-Event-ID` is present, the server first establishes the live subscription, replays matching stored events strictly after that position in ascending order, and then continues with live delivery. Events committed at the history-to-live boundary may be delivered more than once, so consumers should deduplicate by `event_id`. This provides at-least-once delivery across a reconnect without leaving a gap.

Replay is limited to 1000 matching events. An older position returns 409 before the stream opens. Slow consumers are disconnected when the bounded live buffer fills and should reconnect with their last processed id. Concurrent streams are limited per tenant and may return 429 with `Retry-After`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
const response = await client.events.streamEvents();
for await (const item of response) {
    console.log(item);
}

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.StreamEventsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EventsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## timeline
<details><summary><code>client.timeline.<a href="/src/api/resources/timeline/client/Client.ts">getTimeline</a>({ ...params }) -> ChronicleLabsApi.EventPage</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. Cursor paginated, newest first.

Pass `cursor` from `next_cursor` to read the following page, and stop when `has_more` is false. The cursor is opaque: it is a keyset over `(event_time, event_id)`, it is exclusive so a row cannot repeat across pages, and its encoding may change without notice. Do not parse or construct one.

`include_linked=true` selects a different read that also returns causally linked events. That read is not paginated: it returns one page with `has_more` false, and it cannot be combined with `limit` or `cursor`. `since` is only available on that read, because the paginated read has no time filter.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.timeline.getTimeline({
    entity_type: "entity_type",
    entity_id: "entity_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetTimelineRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `TimelineClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## search
<details><summary><code>client.search.<a href="/src/api/resources/search/client/Client.ts">events</a>({ ...params }) -> ChronicleLabsApi.EventListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. The page size is capped at 200 and a cursor can advance through at most 1,000 relevance-ranked results. Each request consumes 5 rate-limit units.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.search.events({
    query: "query"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.SearchRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SearchClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## discover
<details><summary><code>client.discover.<a href="/src/api/resources/discover/client/Client.ts">listSources</a>() -> ChronicleLabsApi.SourceListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. Returns the complete source metadata set without pagination.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.discover.listSources();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `DiscoverClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.discover.<a href="/src/api/resources/discover/client/Client.ts">listEntityTypes</a>() -> ChronicleLabsApi.EntityTypeListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. Returns the complete entity-type metadata set without pagination.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.discover.listEntityTypes();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `DiscoverClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.discover.<a href="/src/api/resources/discover/client/Client.ts">listEntities</a>({ ...params }) -> ChronicleLabsApi.EntityListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. Entities are ordered by event count and entity ID. The limit is capped at 200; pass `next_cursor` as `cursor`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.discover.listEntities({
    entity_type: "entity_type"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListEntitiesRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DiscoverClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.discover.<a href="/src/api/resources/discover/client/Client.ts">getEventSchema</a>({ ...params }) -> ChronicleLabsApi.SourceSchema</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.discover.getEventSchema({
    source: "source",
    event_type: "event_type"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetEventSchemaRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DiscoverClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## links
<details><summary><code>client.links.<a href="/src/api/resources/links/client/Client.ts">addEntityRef</a>({ ...params }) -> ChronicleLabsApi.StatusResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.links.addEntityRef({
    event_id: "event_id",
    entity_type: "entity_type",
    entity_id: "entity_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.AddEntityRefRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `LinksClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.links.<a href="/src/api/resources/links/client/Client.ts">createEventLink</a>({ ...params }) -> ChronicleLabsApi.CreateLinkResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.links.createEventLink({
    source_event_id: "source_event_id",
    target_event_id: "target_event_id",
    link_type: "link_type",
    confidence: 1.1
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateLinkRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `LinksClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.links.<a href="/src/api/resources/links/client/Client.ts">linkEntities</a>({ ...params }) -> ChronicleLabsApi.LinkEntityResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.links.linkEntities({
    from_entity_type: "from_entity_type",
    from_entity_id: "from_entity_id",
    to_entity_type: "to_entity_type",
    to_entity_id: "to_entity_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.LinkEntityRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `LinksClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.links.<a href="/src/api/resources/links/client/Client.ts">traverseGraph</a>({ ...params }) -> ChronicleLabsApi.EventListResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope events:read or events:write. The traversal is bounded by `max_depth`, is not cursor-paginated, and consumes 5 rate-limit units.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.links.traverseGraph({
    start_event_id: "start_event_id",
    direction: "outgoing"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GraphRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `LinksClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## sdk
<details><summary><code>client.sdk.<a href="/src/api/resources/sdk/client/Client.ts">identifyUser</a>({ ...params }) -> ChronicleLabsApi.AcceptedResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope users:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.sdk.identifyUser({
    user_id: "user_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.IdentifyUserRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SdkClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.sdk.<a href="/src/api/resources/sdk/client/Client.ts">trackSignals</a>({ ...params }) -> ChronicleLabsApi.AcceptedResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope signals:write. Maximum 1000 signals per request; larger batches are rejected with 422. Each request consumes 10 rate-limit units.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.sdk.trackSignals();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.TrackSignalsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SdkClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.sdk.<a href="/src/api/resources/sdk/client/Client.ts">trackTraces</a>({ ...params }) -> ChronicleLabsApi.AcceptedResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope traces:write. Maximum 1000 traces or total spans per request; larger batches are rejected with 422. Each request consumes 10 rate-limit units.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.sdk.trackTraces();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.TrackTracesRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SdkClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## agents
<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">listAgents</a>() -> ChronicleLabsApi.AgentSummary[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.listAgents();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">searchAgentHashIndex</a>({ ...params }) -> ChronicleLabsApi.HashIndexEntry[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.searchAgentHashIndex();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.SearchAgentHashIndexRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">subscribeToAgentChanges</a>() -> core.Stream&lt;Record&lt;string, unknown&gt;&gt;</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
const response = await client.agents.subscribeToAgentChanges();
for await (const item of response) {
    console.log(item);
}

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">updateAgent</a>({ ...params }) -> ChronicleLabsApi.AgentSummary</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.updateAgent({
    name: "name"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.UpdateAgentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">getAgentSnapshot</a>({ ...params }) -> ChronicleLabsApi.AgentSnapshot | null</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.getAgentSnapshot({
    name: "name"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetAgentSnapshotRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">pinLatestAgentVersion</a>({ ...params }) -> ChronicleLabsApi.AgentSummary</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.pinLatestAgentVersion({
    name: "name"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.PinLatestAgentVersionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">createAgentChatSession</a>({ ...params }) -> ChronicleLabsApi.CreateAgentChatSessionResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.createAgentChatSession({
    name: "name"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateAgentChatSessionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">getAgentChatSession</a>({ ...params }) -> ChronicleLabsApi.AgentChatSession</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.getAgentChatSession({
    name: "name",
    session_id: "session_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetAgentChatSessionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">sendAgentChatMessage</a>({ ...params }) -> ChronicleLabsApi.SendAgentChatMessageResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.sendAgentChatMessage({
    name: "name",
    session_id: "session_id",
    text: "text"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.SendAgentChatMessageRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">registerAgentArtifact</a>({ ...params }) -> ChronicleLabsApi.AgentVersionSummary</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope agents:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.registerAgentArtifact({
    artifact: {
        artifactId: "artifactId",
        configHash: "configHash",
        framework: "vercel-ai-sdk",
        model: {
            label: "label"
        },
        name: "name",
        provenance: {
            createdAt: "2024-01-15T09:30:00Z"
        },
        schemaVersion: "schemaVersion",
        tools: [{
                name: "name"
            }],
        version: "version"
    }
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.RegisterAgentArtifactRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.agents.<a href="/src/api/resources/agents/client/Client.ts">recordAgentRuns</a>({ ...params }) -> ChronicleLabsApi.RecordAgentRunsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Requires scope agents:write.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.agents.recordAgentRuns({
    runs: [{
            artifactId: "artifactId",
            configHash: "configHash",
            operation: "generate",
            runId: "runId",
            schemaVersion: "schemaVersion",
            startedAt: "2024-01-15T09:30:00Z",
            status: "started",
            toolCalls: [{
                    callId: "callId",
                    startedAt: "2024-01-15T09:30:00Z",
                    status: "started",
                    toolName: "toolName"
                }]
        }]
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.RecordAgentRunsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AgentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## datasets
<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasets</a>({ ...params }) -> ChronicleLabsApi.TaskSuitePage</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasets();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">createDataset</a>({ ...params }) -> ChronicleLabsApi.TaskSuite</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.createDataset({
    name: "name"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateTaskSuitePayload` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">createDatasetWithTrace</a>({ ...params }) -> ChronicleLabsApi.CreateTaskSuiteWithTraceResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.createDatasetWithTrace({
    dataset: {
        name: "name"
    },
    trace: {
        traceId: "traceId"
    }
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateTaskSuiteWithTraceRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">getDataset</a>({ ...params }) -> ChronicleLabsApi.TaskSuiteDetail</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.getDataset({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetDatasetRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">archiveDataset</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.archiveDataset({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ArchiveDatasetRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">updateDataset</a>({ ...params }) -> ChronicleLabsApi.TaskSuite</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.updateDataset({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.TaskSuitePatch` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">getDatasetSnapshot</a>({ ...params }) -> ChronicleLabsApi.TaskSuiteSnapshot</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.getDatasetSnapshot({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetDatasetSnapshotRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetTraces</a>({ ...params }) -> ChronicleLabsApi.TaskPage</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetTraces({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetTracesRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">addTraceToDataset</a>({ ...params }) -> ChronicleLabsApi.AddTaskFromTraceResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.addTraceToDataset({
    dataset_id: "dataset_id",
    traceId: "traceId"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.AddTaskFromTraceRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">updateDatasetTraces</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.updateDatasetTraces({
    dataset_id: "dataset_id",
    patch: {},
    traceIds: ["traceIds"]
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.UpdateTracesRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">removeTraceFromDataset</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.removeTraceFromDataset({
    dataset_id: "dataset_id",
    membership_id: "membership_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.RemoveTraceFromDatasetRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">refreshDatasetTrace</a>({ ...params }) -> ChronicleLabsApi.TaskMembership</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.refreshDatasetTrace({
    dataset_id: "dataset_id",
    membership_id: "membership_id",
    body: {}
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.RefreshDatasetTraceRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetTraceEvents</a>({ ...params }) -> ChronicleLabsApi.TaskEventPage</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetTraceEvents({
    dataset_id: "dataset_id",
    membership_id: "membership_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetTraceEventsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listTraceDatasetMemberships</a>({ ...params }) -> ChronicleLabsApi.TaskMembership[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listTraceDatasetMemberships({
    trace_id: "trace_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListTraceDatasetMembershipsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetTasks</a>({ ...params }) -> ChronicleLabsApi.TaskPage</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetTasks({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetTasksRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">createDatasetTask</a>({ ...params }) -> ChronicleLabsApi.Task</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.createDatasetTask({
    dataset_id: "dataset_id",
    body: {
        "key": "value"
    }
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateDatasetTaskRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">getDatasetTask</a>({ ...params }) -> ChronicleLabsApi.Task</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.getDatasetTask({
    dataset_id: "dataset_id",
    membership_id: "membership_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetDatasetTaskRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">deleteDatasetTask</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.deleteDatasetTask({
    dataset_id: "dataset_id",
    membership_id: "membership_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.DeleteDatasetTaskRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">updateDatasetTask</a>({ ...params }) -> ChronicleLabsApi.Task</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.updateDatasetTask({
    dataset_id: "dataset_id",
    membership_id: "membership_id",
    body: {
        "key": "value"
    }
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.UpdateDatasetTaskRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">setDatasetTaskVerifiers</a>({ ...params }) -> ChronicleLabsApi.Task</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.setDatasetTaskVerifiers({
    dataset_id: "dataset_id",
    membership_id: "membership_id",
    verifiers: [{
            scorerId: "scorerId"
        }]
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.SetTaskVerifiersRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetTaskEvents</a>({ ...params }) -> ChronicleLabsApi.TaskEventPage</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetTaskEvents({
    dataset_id: "dataset_id",
    membership_id: "membership_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetTaskEventsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">refreshDatasetTask</a>({ ...params }) -> ChronicleLabsApi.TaskMembership</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.refreshDatasetTask({
    dataset_id: "dataset_id",
    membership_id: "membership_id",
    body: {}
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.RefreshDatasetTaskRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetClusters</a>({ ...params }) -> ChronicleLabsApi.DatasetCluster[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetClusters({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetClustersRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">createDatasetCluster</a>({ ...params }) -> ChronicleLabsApi.DatasetCluster</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.createDatasetCluster({
    dataset_id: "dataset_id",
    color: "color",
    label: "label"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateClusterRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">deleteDatasetCluster</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.deleteDatasetCluster({
    dataset_id: "dataset_id",
    cluster_id: "cluster_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.DeleteDatasetClusterRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">updateDatasetCluster</a>({ ...params }) -> ChronicleLabsApi.DatasetCluster</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.updateDatasetCluster({
    dataset_id: "dataset_id",
    cluster_id: "cluster_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.UpdateClusterRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetSavedViews</a>({ ...params }) -> ChronicleLabsApi.DatasetSavedView[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetSavedViews({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetSavedViewsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">createDatasetSavedView</a>({ ...params }) -> ChronicleLabsApi.DatasetSavedView</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.createDatasetSavedView({
    dataset_id: "dataset_id",
    name: "name",
    scope: "personal",
    state: {}
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateSavedViewRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">deleteDatasetSavedView</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.deleteDatasetSavedView({
    dataset_id: "dataset_id",
    view_id: "view_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.DeleteDatasetSavedViewRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">updateDatasetSavedView</a>({ ...params }) -> ChronicleLabsApi.DatasetSavedView</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.updateDatasetSavedView({
    dataset_id: "dataset_id",
    view_id: "view_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.DatasetSavedViewPatch` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetVersions</a>({ ...params }) -> ChronicleLabsApi.TaskSuiteVersion[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetVersions({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetVersionsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">publishDatasetVersion</a>({ ...params }) -> ChronicleLabsApi.TaskSuiteVersion</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.publishDatasetVersion({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.PublishVersionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">getDatasetVersion</a>({ ...params }) -> ChronicleLabsApi.TaskSuiteSnapshot</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.getDatasetVersion({
    dataset_id: "dataset_id",
    version_id: "version_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetDatasetVersionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.datasets.<a href="/src/api/resources/datasets/client/Client.ts">listDatasetEvaluationRuns</a>({ ...params }) -> ChronicleLabsApi.TaskSuiteEvalRun[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.datasets.listDatasetEvaluationRuns({
    dataset_id: "dataset_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListDatasetEvaluationRunsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `DatasetsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## environments
<details><summary><code>client.environments.<a href="/src/api/resources/environments/client/Client.ts">listEnvironments</a>() -> ChronicleLabsApi.ListEnvironmentsResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.environments.listEnvironments();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `EnvironmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.environments.<a href="/src/api/resources/environments/client/Client.ts">createEnvironment</a>({ ...params }) -> ChronicleLabsApi.EnvironmentResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.environments.createEnvironment({
    slug: "slug",
    label: "label"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateEnvironmentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EnvironmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.environments.<a href="/src/api/resources/environments/client/Client.ts">getEnvironment</a>({ ...params }) -> ChronicleLabsApi.EnvironmentResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.environments.getEnvironment({
    environment_id: "environment_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetEnvironmentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EnvironmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.environments.<a href="/src/api/resources/environments/client/Client.ts">listEnvironmentVersions</a>({ ...params }) -> ChronicleLabsApi.EnvironmentVersionRecord[]</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.environments.listEnvironmentVersions({
    environment_id: "environment_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListEnvironmentVersionsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EnvironmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.environments.<a href="/src/api/resources/environments/client/Client.ts">createEnvironmentVersion</a>({ ...params }) -> ChronicleLabsApi.EnvironmentVersionResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.environments.createEnvironmentVersion({
    environment_id: "environment_id",
    version: "version"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateEnvironmentVersionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EnvironmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.environments.<a href="/src/api/resources/environments/client/Client.ts">getEnvironmentVersion</a>({ ...params }) -> ChronicleLabsApi.EnvironmentVersionResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.environments.getEnvironmentVersion({
    environment_id: "environment_id",
    version_selector: "version_selector"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetEnvironmentVersionRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EnvironmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.environments.<a href="/src/api/resources/environments/client/Client.ts">compileEnvironmentVersion</a>({ ...params }) -> ChronicleLabsApi.CompileEnvironmentResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.environments.compileEnvironmentVersion({
    environment_id: "environment_id",
    version_selector: "version_selector",
    datasetSnapshotId: "datasetSnapshotId",
    scenarioId: "scenarioId"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CompileEnvironmentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EnvironmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## backtests
<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">getBacktestsAvailability</a>() -> ChronicleLabsApi.BacktestsAvailability</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.backtests.getBacktestsAvailability();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">listBacktestJobs</a>({ ...params }) -> ChronicleLabsApi.ListBacktestJobsResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.backtests.listBacktestJobs();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListBacktestJobsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">createBacktestJob</a>({ ...params }) -> ChronicleLabsApi.CreateBacktestJobResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns 202 after the durable job and its trials have been admitted.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.backtests.createBacktestJob({
    name: "name",
    recipe: {
        agents: [{
                hue: "hue",
                id: "id",
                label: "label",
                notes: "notes"
            }],
        data: {
            kind: "composed",
            scenarios: [{
                    count: 1,
                    id: "id",
                    kind: "adversarial",
                    label: "label"
                }],
            sources: [{
                    count: 1,
                    id: "id",
                    kind: "prod",
                    label: "label"
                }]
        },
        graders: [{
                id: "id",
                kind: "rubric",
                label: "label",
                source: "proposed",
                weight: "low"
            }],
        mode: "replay",
        name: "name"
    }
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateBacktestJobRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">getBacktestJob</a>({ ...params }) -> ChronicleLabsApi.BacktestJobDetailResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.backtests.getBacktestJob({
    job_id: "job_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetBacktestJobRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">listBacktestJobTrials</a>({ ...params }) -> ChronicleLabsApi.ListBacktestJobTrialsResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.backtests.listBacktestJobTrials({
    job_id: "job_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.ListBacktestJobTrialsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">getBacktestTrial</a>({ ...params }) -> ChronicleLabsApi.BacktestTrialDetailResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.backtests.getBacktestTrial({
    job_id: "job_id",
    trial_id: "trial_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.GetBacktestTrialRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">cancelBacktestJob</a>({ ...params }) -> ChronicleLabsApi.CancelBacktestJobResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.backtests.cancelBacktestJob({
    job_id: "job_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CancelBacktestJobRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.backtests.<a href="/src/api/resources/backtests/client/Client.ts">streamBacktestJobEvents</a>({ ...params }) -> core.Stream&lt;ChronicleLabsApi.TrialEvent&gt;</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
const response = await client.backtests.streamBacktestJobEvents({
    job_id: "job_id"
});
for await (const item of response) {
    console.log(item);
}

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.StreamBacktestJobEventsRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BacktestsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## credentials
<details><summary><code>client.credentials.<a href="/src/api/resources/credentials/client/Client.ts">listSdkKeys</a>() -> ChronicleLabsApi.SdkKeyListResponse</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.credentials.listSdkKeys();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `CredentialsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.credentials.<a href="/src/api/resources/credentials/client/Client.ts">createSdkKey</a>({ ...params }) -> ChronicleLabsApi.CreatedSdkKey</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

The bearer secret is returned once and is not stored in plaintext.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.credentials.createSdkKey({
    name: "name",
    scopes: ["traces:write"]
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.CreateSdkKeyRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CredentialsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.credentials.<a href="/src/api/resources/credentials/client/Client.ts">revokeSdkKey</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.credentials.revokeSdkKey({
    key_id: "key_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `ChronicleLabsApi.RevokeSdkKeyRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `CredentialsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

