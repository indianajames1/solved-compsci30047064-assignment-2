Download Link: https://assignmentchef.com/product/solved-compsci3004_7064-assignment-2
<br>
The aim of this assignment is to improve your learning experience in the page replacement algorithms. Following our discussion of paging in lectures, this practical will allow you to explore how real applications respond to a variety of page replacement schemes. Since modifying a real operating system to use different page replacement algorithms can be quite a technical exercise, your task will be to implement a program that simulates the behaviour of a memory system using a variety of paging schemes.

<h2>Memory Traces</h2>

We provide you with some memory traces to assist you developing your simulator.

Each trace is a series of lines, each listing a hexadecimal memory address preceded by R or W to indicate a read or a write. There are also lines throughout the trace starting with a # followed by a process ID and command. For example, a trace file for gcc might start like this:

# gcc

R 0041f7a0

R 13f5e2c0

R 05e78900

R 004758a0

W 31348900

The lines in the trace file beginning with #.

<h2>Simulator Requirements</h2>

Your task is to write a simulator that reads a memory trace and simulates the action of a virtual memory system with a single level page table.

Your memory system should keep track of which pages are loaded into memory.

<ul>

 <li>As it processes each memory event from the trace, it should check to see if the corresponding page is loaded.</li>

 <li>If not, it should choose a victim page in memory to replace.</li>

 <li>If the victim page to be replaced is dirty, it must be saved to disk before replacement.</li>

 <li>Finally, the new page is loaded into memory from disk, and the page table is updated.</li>

</ul>

This is just a simulation of the page table, so you do not actually need to read and write data from disk. When a simulated disk read or write occurs, simply increment a counter to keep track of disk reads and writes, respectively.

You must implement the following page replacement algorithms:

<ul>

 <li>FIFO: Replace the page that has been resident in memory longest.</li>

 <li>LRU: Replace the page that has been resident in memory oldest.</li>

 <li>ARB: Use multiple reference bits to approximate LRU. Your implementation should use an <em>a</em>-bit shift register and the given regular interval <em>b</em>. See textbook section 9.4.5.1.

  <ul>

   <li>If two page’s ARB are equal, you should use FIFO (longest) to replace the frame.</li>

  </ul></li>

 <li>WSARB-1: Combine the ARB page replacement algorithm with the working set model that keeps track of the page reference frequencies over a given window size (page references) for each process (see textbook section 9.6.2). It works as follows:

  <ul>

   <li>Associate each page with <em>a </em>reference bits as the shift register (R) to track the reference pattern over the recent time intervals (for a given time interval length), and an integer counter (C) of 8 bits to record the reference frequency of pages (in the memory frames) in the current working set window. R and C are initialized to 0.</li>

   <li>Page replacement is done by first selecting the victim page of the smallest reference frequency (the smallest value of C), and then selecting the page that has the smallest reference pattern value in the ARB shift register (the smallest value of R) if there are multiple pages with the smallest reference frequency.</li>

  </ul></li>

 <li>WSARB-2: Same as WSARB-1 in combining ARB with the working set model, but selecting a victim for page replacement is done in the reverse order:

  <ul>

   <li>First select the page having the smallest reference patten value in R, and then that of the smallest reference frequency value in C if there are multiple pages with the smallest reference patten value.</li>

  </ul></li>

</ul>

Note that for WSARB-1 and WSARB-2, same as ARB, if there are multiple pages with the same smallest values of R and C, victim will be chosen according to FIFO among them.

<h1>Test</h1>

<h2>Arguments</h2>

Your code will be compiled using following order in your SVN folder.

g++ -std=c++11 PageReplacement.cpp -o PageReplacement The simulator should accept arguments as follows:

<ol>

 <li>The filename of the trace file</li>

 <li>The page/frame size in bytes (we recommend you use 4096 bytes when testing).</li>

 <li>The number of page frames in the simulated memory.</li>

</ol>

The trace provided should be opened and read as a file, **not** parsed as text input from stdin.

For example, your code might be run like this:

“ ./PageReplacement input.txt 4096 32 FIFO ” Where:

<ul>

 <li>The program being run is ‘./PageReplacement’,</li>

 <li>The name of the input file is ‘input.txt’,</li>

 <li>A page is ‘4096’ bytes,</li>

 <li>There are ‘32’ frames in physical memory,</li>

 <li>The page replacement algorithm to use: FIFO / LRU / ARB / WSARB-1 / WSARB-2</li>

</ul>

If the page replacement algorithm is ARB , it should accept the following additional arguments:

<strong>The number of reference bits </strong><em>a </em><strong>(</strong>1 ≤ <em>a </em>≤ 8<strong>) used in the shift register (R).</strong>

<strong>The integer value of regular interval </strong><em>b </em><strong>(</strong>1 ≤ <em>b </em>≤ 12<strong>) for register shifting.</strong>

If the page replacement algorithm is WSARB-1 or WSARB-2, it should accept the following additional argument:

<strong>The number of </strong><em>a</em><strong>, the shift register (R) uses an </strong><em>a</em><strong>-bit shift register (</strong>1 ≤ <em>a </em>≤ 8<strong>).</strong>

<strong>The integer value of regular interval </strong><em>b </em><strong>(</strong>1 ≤ <em>b </em>≤ 12<strong>) for register shifting.</strong>

<strong>The size of the working set window </strong><em>δ</em><strong>, in page references (</strong><em>b </em>≤ <em>δ </em>≤ 256<strong>).</strong>

For example, your code might be run like this:

“

./PageReplacement input.txt 1024 16 ARB 3 3

”

“

./PageReplacement input.txt 4096 32 WSARB-1 8 3 11

”

<h2>Input</h2>

We will provide you with a selection of memory traces to assist you developing your simulator. These will be a mix of specific test cases and real traces from running systems. Each trace is a series of lines, containing two(2) values that represent memory accesses:

<ol>

 <li>A character ‘R’ or ‘W’ that represents whether the memory access is a Read or Writerespectively.</li>

 <li>A hexadecimal memory address.</li>

</ol>

A trace may also contain comment lines, # followed by a process Name.

<strong>An example of a trace:</strong>

# chrome

R 0041f7a0

R 13f5e2c0

R 05e78900

R 004758a0

W 31348900

<h2>Output</h2>

The simulator should run silently with no output until the very end, at which point it prints out a summary like this:

events in trace: 1025 total disk reads: 151 total disk writes: 92 page faults: 151

Where:

<ul>

 <li>“events in trace” is the number of memory accesses in the trace. Should be equal to number of lines in the trace file that start with R or W. Lines starting with # do not count.</li>

 <li>“total disk reads” is the number of times pages have to be read from disk.</li>

 <li>“total disk writes” is the number of times pages have to be written back to disk.</li>

 <li>“page faults” is the number of disk reads in a demand paging system. It may be same with total disk reads in this problem.</li>

</ul>

We will provide a set of expected outputs(on web-submission) to match the given memory traces.