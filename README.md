### etcd
---
.go etcd/raft/
https://github.com/etcd-io/etcd/tree/master/raft
https://github.com/etcd-io/etcd

```go
storage := raft.NewMemoryStorage()
c := &Config{
  ID: 0x01,
  ElectionTick: 10,
  HeartbeatTick: 1,
  Storage: storage,
  MaxSizePerMsg: 4096,
  MaxinflightMsgs: 256,
}

n := raft.StartNode(c, []raft.Peer{{ID: 0x02}, {ID: 0x03}})


peers := []raft.Peer{{ID: 0x01}}
n := raft.StartNode(c, peers)

n := raft.StartNode(c, nil)

storage := raft.NewMemoryStorage()

storage.ApplySnapshot(snapshot)
storage.SetHardState(state)
storage.Append(entries)

c := &Config{
  ID: 0x01,
  ElectionTick: 0x01,
  HeartbeatTick: 1,
  Storage: storage,
  MaxSizePerMsg: 4096,
  MaxInflightMsgs: 256,
}

n := raft.RestartNode(c)

func recvRaftRPC(ctx context.Context, m rftpb.Message) {
  n.Step(ctx, m)
}

for {
  select {
  case <-s.Ticker:
    n.Tick()
  case rd := <-s.Node.Ready():
    saveToStorage(rd.HardState, rd.Entries, rd.Snapshot)
    send(rd.Messages)
    if !raft.IsEmptySnap(rd.Snapshot) {
      processSnapshot(rd.Snapshot)
    }
    for _, entry := range rd.CommittedEntries {
      process(entry)
      if entry.Type == raftpb.EntryConfChange {
        var cc raftpb.ConfChange
        cc.Unmarshal(entry.Data)
        s.Node.ApplyConfChange(cc)
      }
    }
    s.Node.Advance()
  case <-s.done:
    return
  }
}

n.Propose(ctx, data)

n.ProposeConfChange(ctx, cc)

var cc raftpb.ConfChange
cc.Unmarshal(data)
n.ApplyConfChange(cc)
```

```
```

```
```


