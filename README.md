**MPP THIRD TASK. ASSEMBLY BROWSER**

Here is implemented a graphical utility using WPF to analyze the size ratio of files and directories within a selected directory in multi-threaded mode.

**File and directory size analysis**

* Analysis of the size of files and directories must be performed in multi-threaded mode using the system's thread pool and queue.
* Each directory is processed on a separate thread. Processing includes summing the sizes of nested files and queuing all nested directories for similar processing.
* The maximum number of involved threads should be limited (it can be a constant in the code) without changing the settings of the system thread pool (ThreadPool.SetMaxThreads is not allowed).

The thread processing a directory does not need to wait for all nested directories to be processed, but only queue their processing. Otherwise, if the nesting level is high, the threads will sit idle waiting for threads running for nested directories to finish.

Also, when the limit on the number of involved threads is set to a value that is less than the level of nesting of directories in the scanned directory, the program may "hang" due to infinite waiting (mutual blocking), when all threads are busy waiting for the completion of traversal of nested directories, for which there are no threads to start processing. remains.

To solve this problem, the recalculation of the sizes of directories, taking into account nested directories, must be performed separately, after the sizes of all files have been calculated.
