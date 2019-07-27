**Parallelism** (song song) consists of (bao gồm) performing (thực hiện) multiple operations (đa tác vụ) at the same time. **Multiprocessing** is a means to effect parallelism, and it entails (đòi hỏi) spreading tasks over a computer’s central processing units (CPUs, or cores). Multiprocessing is well-suited for CPU-bound tasks: tightly bound (ràng buộc chặt chẽ) `for` loops and mathematical computations (tính toán toán học) usually fall into this category.

**Concurrency** (đồng thời) is a slightly broader (hơi rộng hơn) term (thuật ngữ) than parallelism. It suggests that multiple tasks (đa tác vụ) have the ability (có khả năng) to run in an overlapping manner (cách chồng chéo). (There’s a saying that concurrency does not imply (không bao gồm) parallelism.)

**Threading** (phân luồng) is a concurrent execution model (mô hình thực thi đồng thời) whereby (theo đó) multiple [threads](https://en.wikipedia.org/wiki/Thread_(computing)) take turns executing (lần lượt thực thi) tasks. One process can contain multiple threads. Python has a complicated relationship (mối quan hệ phức tạp) with threading thanks to (nhờ vào) its [GIL](https://realpython.com/python-gil/), but that’s beyond the scope of (vượt quá phạm vi của) this article.

What’s important to know (điều quan trọng cần biết) about threading is that it’s better for IO-bound tasks (các tác vụ ràng buộc IO). While a CPU-bound task is characterized by (đặc trưng bởi) the computer’s cores continually working hard (liên tục làm việc chăm chỉ) from start to finish, an IO-bound job is dominated by (bị chi phối bởi) a lot of waiting on input/output to complete.

To recap the above (để tóm tắt lại những điều ở trên), concurrency encompasses (bao gồm) both multiprocessing (ideal (lý tưởng) for CPU-bound tasks) and threading (suited for IO-bound tasks). Multiprocessing is a form of (một hình thức) parallelism, with parallelism being a specific type (một loại cụ thể) (subset (tập hợp con)) of concurrency. The Python standard library has offered (đã cung cấp) longstanding (từ lâu) [support for both of these](https://docs.python.org/3/library/concurrency.html) through (thông qua) its `multiprocessing`, `threading`, and `concurrent.futures` packages.

The `asyncio` package [is billed by](https://www.ldoceonline.com/dictionary/be-billed-to-do-something) (được quảng cáo bởi) the Python documentation as [a library to write concurrent code](https://docs.python.org/3/library/asyncio.html). However, async IO is not threading, nor is it multiprocessing. It is not built on top of  (không được xây dựng trên) either of these (một trong hai).

In fact (thực tế), async IO is a single-threaded (đơn luồng), single-process (đơn tiến trình) design: it uses **cooperative multitasking** (hợp tác đa nhiệm), a term that you’ll flesh out (đưa ra) by the end (vào cuối) of this tutorial. It has been said in other words that (nói một cách khác) async IO gives a feeling of (mang lại cảm giác) concurrency despite (mặc dù) using a single thread in a single process. Coroutines (a central feature of async IO) can be scheduled concurrently (lên lịch đồng thời), but they are not inherently (vốn dĩ) concurrent.

To reiterate (nói cách khác), async IO is a style of concurrent programming (phong cách lập trình đồng thời), but it is not parallelism. It’s more closely aligned with (liên kết chặt chẽ hơn với) threading than with multiprocessing but is very much distinct from (nhưng rất nhiều khác biệt với) both of these and is a standalone member (thành viên độc lập) in concurrency’s bag of tricks (thủ thuật).

That leaves one more term (một thuật ngữ nữa). What does it mean for something to be **asynchronous** (bất đồng bộ)? This isn’t a rigorous (khắt khe) definition (định nghĩa), but for our purposes (mục đích) here, I can think of two properties (thuộc tính):

- Asynchronous [routines](http://tratu.soha.vn/dict/en_vn/Routine) (đoạn chương trình) are able to (có thể) “pause” while waiting (trong khi chờ đợi) on their ultimate result (kết quả cuối cùng) and let other routines run in the meantime.
- Asynchronous code, through the mechanism above (thông qua cơ chế trên), facilitates (tạo điều kiện) concurrent execution. To put it differently (nói cách khác), asynchronous code gives the look and feel of concurrency.

Here’s a diagram to put it all together. The white terms represent (đại diện) concepts (khái niệm), and the green terms represent ways (cách thức) in which they are implemented (thực hiện) or effected (ảnh hưởng):

![img](https://files.realpython.com/media/Screen_Shot_2018-10-17_at_3.18.44_PM.c02792872031.jpg)

I’ll stop there on the comparisons (so sánh) between concurrent programming models. This tutorial is focused on the subcomponent (thành phần con) that is async IO, how to use it (cách sử dụng), and the APIs that have sprung up (xuất hiện) around it. For a thorough exploration of (để tìm hiểu kĩ về) threading versus (so với) multiprocessing versus async IO, pause here and check out Jim Anderson’s [overview of concurrency in Python](https://realpython.com/python-concurrency/). Jim is way funnier (vui tính hơn) than me and has sat in more meetings than me, to boot.
