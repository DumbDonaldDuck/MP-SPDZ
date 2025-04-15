## Topology Extension: Ring and Rotated Star

We have extended the [MP-SPDZ](https://github.com/data61/MP-SPDZ) framework by adding support for two new secret-sharing reconstruction topologies: **ring** and **rotated star**. The official handbook for MP-SPDZ can be found at [MP-SPDZ/README.md](https://github.com/data61/MP-SPDZ/blob/master/README.md).
 This section focuses on how to use these two new topologies with our update.

### Compilation

The compilation procedure remains **identical** to the original MP-SPDZ workflow. No additional steps are required.

### Running the Protocol

When evaluating circuits in the **tree topology**, use the following command:

```shell
./<protocol> -F -N <count> -p <idx> -ip <ipfile> --batch-size <batchsize> -v -s <degree1> -mb <degree2> <circuit>
```

**Descriptions:**

- `<protocol>`: The protocol binary to execute (e.g., `semi2k-party.x`, `spdz2k-party.x`).
- `<count>`: The total number of parties.
- `<idx>`: The index of this party (starting from 0).
- `<ipfile>`: Path to a file listing the IP addresses of all parties, where line $i$ contains the IP address of party $i$ (starting from 0). This file must be identical and shared across all parties.
- `<batchsize>`: The maximum number of secrets that can be reconstructed in parallel. This should be set as high as possible. Recommended: `1000000`.
- `<degree1>`: The tree degree used for aggregating shares toward the root.
- `<degree2>`: The tree degree used for broadcasting results from the root.
- `<circuit>`: The circuit file to evaluate.

**Note:** Each party must execute this command individually using its own `<idx>` value.

------

### Star Topology

```shell
./<protocol> -F -N <count> -p <idx> -ip <ipfile> --batch-size <batchsize> -v -s <degree> <circuit>
```

- `<degree>` should be significantly larger than `<count>`.
  -  **Recommended value:** `100000000000`.

------

### P2P Topology

```shell
./<protocol> -F -N <count> -p <idx> -ip <ipfile> --batch-size <batchsize> -v -d <circuit>
```

------

### Ring and Rotated Star Topologies

The `-s <degree>` flag is used to select between **ring** and **rotated star** topologies:

```shell
./<protocol> -F -N <count> -p <idx> -ip <ipfile> --batch-size <batchsize> -v -s <degree> <circuit>
```

- **Ring topology**: `<degree>` must be **odd** and **smaller than `<count>`**.
  -  **Recommended value:** `1001`.
- **Rotated star topology**: `<degree>` must be **even** and **smaller than `<count>`**.
  -  **Recommended value:** `1002`.
