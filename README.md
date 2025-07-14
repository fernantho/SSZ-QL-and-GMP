## SSZ-QL and GMP
This is a Jupyter Notebook playground for an in-depth thought of our SSZ-QL and GMP design. This README.md provides the questions that arise after the PoC.

---

### Open questions to be considered:
**1. How to specify the path** (This is not an open question as we already chose the one that is implemented. This is just a reminder of the other ways to specify the path).

**Note:** The following are different ways to represent the Path, for consideration during spec finalization.  
Source: [Ethereum consensus-specs issue #2179](https://github.com/ethereum/consensus-specs/issues/2179#issuecomment-759305714)

- An `anchor_type`, `Sequence[Union[int, SSZVariableName]]` tuple (like now, but abstracted away)  
- An `anchor_type`, `GeneralizedIndex` tuple (what Go/Rust implementations may prefer)  
- A literal URI string (more readable, less performant)  
- Some kind of pointer structure  
- Something precomputed
--- 
**2. How to specify single vs multirproof**

If a query request different fields and `includeProofs` is set to `True`, should we provide single proofs per field or just a multiproof?

--- 
**3. How to specify the response**

Should we include de path in the response on a per field basis? or should just specify that response data is ordered following the query paths?

```diff
SZ-QL response (ordered data reply): 
 {
  "version": "electra",
  "execution_optimistic": true,
  "finalized": true,
  "data": [The project scope is cristal clear but we still need to think about different aspects of the project in order to close the specification. 
  
    {
      "value": "0x4b363db94e286120d76eb905340fdd4e54bfe9f06bf33ff6cf5ad27f511bfe95"
    },
    {
      "value": "0x05000000"
    }
  ]
}
SSZ-QL response (explicit): 
 {
  "version": "electra",
  "execution_optimistic": true,
  "finalized": true,
  "data": [
    {
+      "path": ".fork.current_version",
      "value": "0x05000000"
    },
    {
+      "path": ".genesis_validators_root",
      "value": "0x4b363db94e286120d76eb905340fdd4e54bfe9f06bf33ff6cf5ad27f511bfe95"
    }
  ]
}
```

---
**4. Merkle multiproofs, sparse merkle multiproofs or compact merkle multiproofs?**

In this Jupyter Notebook, we use just multiproofs. For the final implementation we must use more efficient proofs.

---


### Getting started
#### Installation
```
python3 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
```