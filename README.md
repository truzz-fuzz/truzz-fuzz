# Path Transitions Tell More: Optimizing Fuzzing Schedules via Runtime Program States
Truzz is a lightweight fuzzing framework to optimize existing CGFs. 
Truzz identifies the bytes related to the validation checks (i.e., the checks guarding error-handling code), and protects those bytes from being frequently mutated, making most generated inputs examine the functionalities of programs, in lieu of being rejected by validation checks. 
The byte-wise relationship determination mitigates the problem of loading extra bytes when fuzzers infer the byte-constraint relation. 
Furthermore, the proposed path transition within Truzz can efficiently prioritize the seed as the new path, harvesting many new edges, likely belongs to a code region with many undiscovered code lines. 
Here, We use NEUZZ to illustrate how to implement TRUZZ.

# truzz_neuzz
NEUZZ models relations between input bytes and branch behaviors by utilizing neural networks. Truzz (NEUZZ) performs a premutation before the mutation strategy proposed by NEUZZ.
## Build
```bash
    gcc -O3 -funroll-loops ./neuzz.c -o neuzz
```
## Usage
Same steps as NEUZZ. Open a terminal, start nn module.
```bash
    python nn.py [program [arguments]]
```
open another terminal, start neuzz module.
```bash
    ./neuzz -i in_dir -o out_dir -l mutation_len [program path [arguments]] @@
```
If you want to try truzz_neuzz on a new program, follow the same steps as NEUZZ.
1. Compile the new program from source code using afl-gcc.
2. Collect the training data by running AFL on the binary for a while(about an hour), then copy the queue folder to neuzz_in.
3. Follow the above two steps to start NN module and NEUZZ module.

# Limitations
1. Except for NEUZZ, the fuzzers used in evaluation all use a deterministic stage.
2. To achieve better performance, TRUZZ relies on the deterministic stage to perform byte analysis. If a fuzzer does not have the deterministic stage, TRUZZ needs to add a pilot stage to analyze the validation-related bytes, which decreases the efficiency.
3. Byte analysis identifies the validation-related bytes only based on path length. However, there is more information that could be used in the fuzzing campaigns. If a program has complex logic in error-handling code, our byte analysis can not identify the validation-related bytes correctly. We can use other information to solve this problem.
4. Byte analysis is more useful for inference-based fuzzers.
5. For the switch-case and checksum, we can not handle them perfectly.

# Future Works
1. We will add more information to improve the accuracy of byte analysis, such as the essential variables' values.
2. If there is no deterministic stage, We will come up with a new way to avoid introducing too much overhead.
3. We will make it more scalable, not limited to inference-based fuzzers.