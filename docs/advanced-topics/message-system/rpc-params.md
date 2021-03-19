---
id: rpc-params
title: RPC Params 
sidebar_label: RPC Params  
---


Both `ServerRpc` and `ClientRpc` methods can be configured either by [ServerRpc]/[ClientRpc] attributes at compile-time and/or `ServerRpcParams`/`ClientRpcParams` at runtime.

Developers can put `ServerRpcParams`/`ClientRpcParams` as the last parameter (optionally) and they could be used for a consolidated space for `XXXRpcReceiveParams` and `XXXRpcSendParams`.

The network framework will inject the corresponding `XXXRpcReceiveParams` to what the user had declared in code when invoked by network receive handling (framework code) and will consume `XXXRpcSendParams` when invoked by RPC send call (user code).


## ServerRpc Params

``` csharp
struct ServerRpcSendParams
{
}

struct ServerRpcReceiveParams
{
    // who sent the RPC?
    ulong SenderClientId;
}

struct ServerRpcParams
{
    ServerRpcSendParams Send;
    ServerRpcReceiveParams Receive;
}


// Both ServerRPC methods below are fine, `ServerRpcParams` is completely optional

[ServerRpc]
void AbcdServerRpc(int somenumber) { /* ... */ }

[ServerRpc]
void XyzwServerRpc(int somenumber, ServerRpcParams rpcParams = default) { /* ... */ }
```

## ClientRpc Params

```csharp
struct ClientRpcSendParams
{
    // who are the target clients?
    ulong[] TargetClientIds;
}

struct ClientRpcReceiveParams
{
}

struct ClientRpcParams
{
    ClientRpcSendParams Send;
    ClientRpcReceiveParams Receive;
}


// Both ClientRPC methods below are fine, `ClientRpcParams` is completely optional

[ClientRpc]
void AbcdClientRpc(int framekey) { /* ... */ }

[ClientRpc]
void XyzwClientRpc(int framekey, ClientRpcParams rpcParams = default) { /* ... */ }
```