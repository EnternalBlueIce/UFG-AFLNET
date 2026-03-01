# UFG-AFLNET: A Greybox Fuzzing Framework with Fine-Grained State Modeling and Gradient-Guided Mutation for Network Protocols
Greybox fuzzing has become an effective technique for uncovering vulnerabilities in network protocol implementations. However, existing approaches still face several significant challenges: (1) state modeling is overly coarse-grained, failing to accurately capture subtle state transitions during protocol execution, (2) focusing solely on the first mutation point that reaches the target state, overlooking other regions that may equally impact the target state, (3) neglecting the non-uniform contribution of different message regions to path coverage.

To address these issues, we propose UFG-AFLNET, a unified greybox fuzzing framework. UFG-AFLNET significantly enhances fuzzing efficiency and vulnerability discovery through fine-grained state machine modeling, gradient-guided sequence selection, and lightweight dynamic taint inference. Specifically, UFG-AFLNET introduces a state clustering learner that uses the Single-Pass clustering algorithm to extend response state machines into fine-grained path state machines, thereby enabling more precise state differentiation. Additionally, to select the most critical mutation points, we employ a recurrent neural network to compute the sensitivity gradients between target path states and message regions. Finally, we use a message sequence mutator supported by dynamic taint inference to assign weights to each byte and prioritize mutations on those most likely to expose new execution paths. Experiments on five widely used protocol implementations show that UFG-AFLNET significantly outperforms baseline algorithms in path coverage, time-to-first-crash, and the number of vulnerabilities discovered. These results demonstrate the potential of UFG-AFLNET in advancing the field of network protocol security testing.
# Install Valgrind (optional):
sudo apt-get update
sudo apt-get install -y valgrind
# Directory Layout (as used in our command)

Seed corpus (RTSP request sequences):

/home/nsas2020/fuzz/AFLNetStatusNeu/tutorials/live555/in-rtsp

Dictionary:

/home/nsas2020/fuzz/AFLNetStatusNeu/tutorials/live555/rtsp.dict

Target server:

/home/nsas2020/fuzz/targetProcess/live555/testProgs/testOnDemandRTSPServer

Output directory:

out-live555 (will be created automatically)

# Usage
./afl-fuzz -d \
  -i /home/nsas2020/fuzz/AFLNetStatusNeu/tutorials/live555/in-rtsp \
  -o out-live555 \
  -N tcp://127.0.0.1/8554 \
  -x /home/nsas2020/fuzz/AFLNetStatusNeu/tutorials/live555/rtsp.dict \
  -P RTSP \
  -D 10000 \
  -q 3 \
  -s 3 \
  -E \
  -K \
  -R /home/nsas2020/fuzz/targetProcess/live555/testProgs/testOnDemandRTSPServer 8554

# Option notes (only the key ones)

-i: seed corpus directory (recorded RTSP message sequences)

-o: output directory

-N tcp://127.0.0.1/8554: target endpoint

-x: protocol dictionary

-P RTSP: protocol parser mode

-D 10000: server initialization wait time (microseconds)

-q 3, -s 3: state/seed selection algorithm IDs

-E: enable state-aware mode

-K: gracefully terminate server (SIGTERM) after a sequence

-R <server> <port>: run the server under the fuzzer’s control

# Run fuzzing under Valgrind (debugging)

Valgrind is useful for diagnosing memory errors and getting richer reports, but it can significantly reduce fuzzing throughput.
valgrind ./afl-fuzz -d \
  -i /home/nsas2020/fuzz/AFLNetStatusNeu/tutorials/live555/in-rtsp \
  -o out-live555 \
  -N tcp://127.0.0.1/8554 \
  -x /home/nsas2020/fuzz/AFLNetStatusNeu/tutorials/live555/rtsp.dict \
  -P RTSP \
  -D 10000 \
  -q 3 \
  -s 3 \
  -E \
  -K \
  -R /home/nsas2020/fuzz/targetProcess/live555/testProgs/testOnDemandRTSPServer 8554


