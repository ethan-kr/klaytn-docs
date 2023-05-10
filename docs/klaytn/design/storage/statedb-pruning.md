# StateDB Pruning <a id="statedb-pruning"></a>

StateDB Pruning은 State Migration과 동일한 문제(state trie의 데이터가 쌓이기만 한다)를 해결하고자 만들어진 기술입니다.

StateDB Pruning은 State Trie의 내부 Hash 중복 참조문제를 해결하고자 32 byte Hash 이후에 6 byte의 uniq한 serial index를 추가해서 Hash 중복 참조문제를 해결했습니다. 
```
Hash : Keccack256 - 32 Byte Hash key
ExtHash : Keccak256 - 32 byte Hash Key + 6 byte Serial index
```

State Trie 내부에서 ExtHash를 사용해 Hash 중복 참조문제를 해결하고, State Root 값을 얻을 때는 ExtHash에서 추가된 6 byte Serial index를 제거하고 Hash를 계산해 기존과 동일한 State Root값을 얻을 수 있어, 하위호환도 가능합니다. 관련 문서 [ medium link ]
 
StateDB Pruning은 정보가 변경된 후 24시간이 지나면 데이터를 삭제하기 때문에 과거 블록의 Account정보를 조회와 관련된 기능은 지원되지 않습니다. 따라서 StateDB Pruing은 CN, PN노드에 적합한 스토리지 운영 방식입니다. 운영하는 노드의 용도를 잘 고려해서 StateDB Pruning기능 활성화를 고민하셔야 합니다.

StateDB Pruning을 이용하기 위해서는 다음과 같은 환경을 구축해야 합니다.
1. klaytn v1.11.0 이상의 바이너리를 사용해야 함.
2. ExtHash 구조체로 인해 DB를 다운받아 sync하는 방법만 존재합니다. 다음 링크에서 DB를 다운받아 주세요.
3. 다음을 참조하여 DB위치를 설정하고 노드를 재시작 [https://ko.docs.klaytn.foundation/content/operation-guide/chaindata-change]

