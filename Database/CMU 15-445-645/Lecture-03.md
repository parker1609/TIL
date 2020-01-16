# Lecture #03: Database Storage (Part I)
- [원본](https://15445.courses.cs.cmu.edu/fall2019/notes/03-storage1.pdf)

## 1. Storage
We will focus on a "disk-oriented" DBMS architecture that primary storage location of the database is on non-volatile disk

At the top of the storage hierarchy you have the devices that are closest to the CPU. This is the fastest storage but it is also the smallest and most expensive. The further you get away from the CPU, the storage devices have larger the capacities but are mush slower and farther away from the CPU. These devices also get cheaper per GB.

### Volatile Devices
- Volatile means that if you pull the power from the machine, then the data is lost.
- Volatile storage supports fast random access with byte-addressable locations. This means that the program can jump to any byte address and get the data that is there.
- For out purposes, we will always refer to this storage class as "memory".

### Non-Volatile Devices
- Non-volatile means that the storage device does not need to be provided continuous power in order for the device to retain the bits that it is storing.
- It is also block/page addressable. This means that in order to read a value at a particular offset, the program first has to load the 4KB page into memory that holds the value the program wants to read.
- Non-volatile storage is traditionally better at sequential access(reading multiple chunks of data at the same time)
- We will refer to this as "disk". We will not make a (major) distinction between solid-state storage(SSD) or spinning hard drives(HDD).

There is also a new class of storage devices that are coming out soon called *non-volatile memory*. These devices are designed to be the best of both worlds: almost as fast as DRAM but with the persistence of disk. We will not cover these devices.

Since the system assumes that the database is stored on disk, the components of the DBMS are responsible for figuring out how to move the data back and forth from the non-volatile disk and volatile memory, since the system cannot operate on the data directly on disk.

We will focus on how we can hide the latency of the disk rather than focusing on optimizations with registers and caches, since getting data from disk is so slow. If reading data from the L1 cache reference took half a second, then reading from an SSD would take 1.7 days and reading from an HDD would take 16.5 weeks.


## 2. Disk-Oriented DBMS Overview
The database is all on disk, and the data in the database files is organized into pages, and the first page is the directory page. In order to operate on the data the DBMS needs to bring the data into memory. It does this by having a *buffer pool* that manages the movement back and forth between disk and memory. The DBMS also have an execution engine that will execute queries. The execution engine will ask the buffer pool for a specific page, and the buffer pool will take care of bringing that page into memory and giving the execution engine a pointer to the page in memory. The buffer pool manager will ensure that the page is there while the execution engine is operating on that memory.


## 3. DBMS vs. OS
A high-level design goal of the DBMS is to support databases that exceed the amount of memory available. Since reading/writing to disk is expensive, is must be managed carefully. We do not want large stalls from fetching something from disk to slow down everything else. So we want the DBMS to be able to process other queries while is is waiting to get the data from disk.

This high-level design goal is like virtual memory, where there is a large address space and a place for the OS to bring in pages from disk.

One way to achieve this virtual memory, is by using *mmap* to map the contents of a file in a process address space, which makes the OS responsible for moving pages back and forth between disk and memory. Unfortunately, this means that if *mmap* hits page fault, this will block the process.

- You never want to use *mmap* in your DBMS if you need to write.
- The DBMS (almost) always wants to control things itself and can do a better job at it since it knows more about the data being accessed and the queries being processed.
- The operating system is not your friend.

It is possible th use the OS by using.

- *madvise*: Tells the OS know when you are planning on reading certain pages.
- *mlock*: Tells the OS to not swap memory ranges out to disk.
- *msync*: Tells the OS to flush memory ranges out to disk.

We do not advise using *mmap* in a DBMS for correctness and performance reasons.

Even though the system will have functionalities that seem like something the OS can provide, Having the DBMS implement these procedures itself gives it better control and performance.


## 4. File Storage
In its most basic form, a DBMS storage a database as files on disk. Some may use a file hierarchy, others may use a single file (e.g., SQLite).

The OS does not know anything about the contents of these files. Only the DBMS knows how to decipher their contents, since it is encoded in a way specific to the DBMS.

The DBMS's *storage manager* is responsible for managing a database's files. It represents the files as a collection of pages. It also keeps track of what data has been read and written to pages, as well how much free space there is in the pages.


## 5. Databse Pages
The DBMS organizes the database across one or more files in fixed-size blocks of data called *pages*. Pages can contain different kinds of data (tuples, indexes, etc). Most systems will not mix these types within pages. Some systems will require that it is self-contained, meaning that all the information needed to read each page is on the page itself.

Each page is given a unique identifier. If the database is a single file, then the page id can just be the file offset. Most DBMSs have an indirection layer that maps a page id to a file path and offset. The upper levels of the system will ask for a specific page number and then the storage manager will have to turn that page number into a file and an offset to find the page.

Mos DBMSs uses fixed-size pages to avoid the engineering overhead needed to support variable-sized pages. For example, with variable-size pages, deleting a page could create a hole in files that hte DBMS cannot easily fill with new pages.

There are three concepts of pages in DBMS:
1. Hardware page (usually 4KB)
2. OS page (4KB)
3. Database page (1-16KB)

The storage device guarantees an atomic write of the size of the hardware page. If the hardware page is 4KB, then when the system tries to write 4KB ot the disk, either all 4KB will be written, or none of it will. This means that if our database page is larger than our hardware page, the DBMS will have to take extra measures to ensure that the data gets written out safety since the program can get partway through writing a database page to disk when the system crashes.


## 6. Database Heap
There are a couple ways to find the location of the page a DBMS wants on the disk, and a heap file organization is one of the ways.

A *heap file* is an unordered collection of pages where tuples are stored in random order.

The DBMS anc locate a page on disk given a *page_id* by using a linked list of pages or a page directory.

1. **Liked List**: Header page holds pointers to to a list of free pages and a list of data pages. However, if the DBMS is looking for a specific page, it has to do a sequential scan on the data page list until it finds the page it is looking for.
2. **Page Directory**: DBMS maintains special pages that track locations of data pages along with the amount of free space on each page.


## 7.Page Layout
Every page includes a header that records meta-data about the page's contents:
- Page size.
- Checksum.
- DBMS version.
- Transaction visibility.
- Some systems require pages to be self-contained(e.g oracle)

A strawman approach to laying out data is to keep track of how many tuples the DBMS has stored in a page and then every time it adds a new tuple, it appends the tuple to the end. However, problems arise when it delete a tuple or when the tuples have variable-length attributes.

These are two main approaches to laying out data in pages: (1) slotted-pages and (2) log-structured.

**Slotted-Pages**: Page maps slots to offsets.
- Most common approach used in DBMSs today.
- Header keeps track of the number of used slots and the offset of the starting location of last used slot and a slot array, which keeps track of the location of the start of each tuple.
- To add a tuple, the slot array will grow from the beginning to the end, and the data of the tuples will grow from end to the beginning. The page is considered full when the slot array and the tuple data meet.

**Log-Structured**: Instead of storing tuples, the DBMS only stores log records.
- Stores records to file of how the database was modified (insert, update, deletes)
- To read a record, the DBMS scans the log file backwards and "recreates" the tuple.
- Fast writes, potentially slow reads.
- Works well on append-only storage because the DBMS cannot go back and update the data.
- To avoid long reads the DBMS can have indexes to allow it to jump to specific locations in the log. It can also periodically compact the log (if it had a tuple and then made and update to it, it could compact it down to just inserting the updated tuple). The issue with compaction is the DBMS ends up with write amplification (it re-writes the same data over and over again)


## 8. Tuple Layout
A tuple is essentially a sequence of bytes. It is DBMS's job to interpret those bytes into attribute types and values.

**Tuple Header**: Contains meta-data about the tuple.
- Visibility information for the DBMS's concurrency control protocol(i.e., information about which transaction created/modified that tuple).
- Bit Map for *NULL* values.
- Not that the DBMS does not need to store meta-data about the schema of the database here.
**Tuple Data**: Actual data for attributes.
- Attributes are typically stored in the order that you specify them when create the table.
- Most DBMSs do not allow a tuple to exceed the size of a page.
**Unique Identifier**:
- Each tuple in the database is assigned a unique identifier.
- Most common: *page_id + (offset of slot)*
- An application **cannot** rely on these ids to mean anything.

**Denormalized Tuple Data**: If two tables are related, the DBMS can "pre-join" them, so the tables and up on the same page. This makes reads faster since the DBMS only has to load in one page rather than two separate pages, but it makes updates more expensive since the DBMS needs more space for each tuple.



## 1. Storage
이 강의는 "disk-oriented" DBMS 아키텍처에 초점을 맞출 예정이며, DBMS의 데이터베이스 기본 스토리지 위치는 비 휘발성(non-volatile) 디스크에 있다.

[3-1]

스토리지 계층의 맨 위가 CPU와 가장 가까운 장치이다. 이 위치의 스토리지는 빠르지만 용량이 작고 비싸다. CPU에서 멀어질수록 스토리지 장치의 용량은 커지고 GB당 가격은 싸지만 훨씬 느려진다.

- **휘발성 장치(Volatile Devices)**:
    - 휘발성은 기계의 전원을 끄면 데이터가 손실되는 것을 의미한다.
    - 휘발성 스토리지는 바이트 단위로 주소가 지정된 위치에서 빠른 랜덤 액세스를 지원한다. 이는 프로그램이 어떤 바이트 주소로 바로 갈 수 있고, 거기에 있는 데이터를 얻을 수 있다는 의미이다.
    - 이후부터 이 스토리지 종류를 "메모리"라고 할 것이다.
- **비휘발성 장치(Non-Volatile Devices)**:
    - 비휘발성은 저장 장치의 비트 데이터를 유지하기 위해 계속해서 전원을 공급할 필요가 없다.
    - 비휘발성은 block/page 주소 형식으로 접근가능하다. 특정 오프셋(offset)에서 값을 읽으려면 해당 값이 포함된 프로그램을 4KB 페이지 단위로 메모리에 로드해야한다.
    - 비휘발성 스토리지는 전통적으로 순차적으로 접근하는 것이 효율적이다.(동시에 여러 chunk 데이터를 읽는다.)
    - 이후부터 이 스토리지를 "디스크"라고 부르며, SSD와 HDD는 같은 비휘발성이므로 자세한 차이점에 대해서는 생각하지 않도록 한다.

현재 비휘발성 장치 중에는 DRAM만큼 빠른 새로운 저장 장치도 나오고 있다. 하지만 이 강의에서는 여기까지 취급하지는 않을 것이다.

데이터베이스는 디스크에 저장되며, DBMS는 데이터베이스에 저장된 데이터를 비휘발성 디스크와 휘발성 메모리 사이를 이동시키는 책임이 있다. 데이터베이스 자체가 직접적으로 데이터를 운영할 수는 없다.

[3-2]

디스크는 속도가 매우 느리므로 캐시와 레지스터를 사용하여 최적화하기보다는  디스크의 응답시간을 어떻게 줄일 수 있는지에 대해 초점을 맞출 것이다. 만약 L1 캐시에서 데이터를 읽는데 걸리는 시간이 0.5초라고 가정하면, SSD는 1.7일이 걸리고 HDD는 16.5 주가 걸릴 것이다.


## 2. 디스크 지향 DBMS 개요
데이터베이스는 모두 디스크에 있으며, 데이터베이스 파일의 데이터는 페이지로 구성되며, 첫 페이지는 디렉터리 페이지다. DBMS는 데이터를 메모리에 저장해야 한다. 디스크와 메모리 사이의 이동을 관리하는 *버퍼 풀*을 보유하여 이 작업을 수행한다. DBMS에는 쿼리를 실행할 실행 엔진도 있다. 실행 엔진은 버퍼 풀에 특정 페이지를 요청하고, 버퍼 풀은 그 페이지를 메모리에 가져오고 실행 엔진에게 메모리의 페이지로 포인터를 부여하는 일을 처리할 것이다. 버퍼 풀 관리자는 실행 엔진이 해당 메모리에서 작동하는 동안 페이지가 있는지 확인할 것이다.


## 3. DBMS vs. OS
DBMS의 설계 목표는 사용 가능한 메모리보다 많은 양의 데이터를 처리하는 것이다. 디스크에 대한 읽기/쓰기 비용이 많이 들기 때문에 주의 깊게 관리해야 한다. 우리는 디스크로부터 큰 데이터를 읽어오는 동안 다른 모든 기능들이 느려지면 안된다. 따라서 DBMS가 디스크에서 데이터를 가져오는 동안 다른 쿼리를 실행할 수 있어야 한다.

이 설계 목표는 운영체제의 가상 메모리와 같은 것으로, 여기에는 넓은 주소 공간과 운영체제가 디스크에서 페이지를 가져올 수 있는 장소가 있다.

가상 메모리로 만드는 한 가지 방법은 *mmap*을 사용하여 프로세스 주소 공간에 있는 파일의 내용을 매핑하는 것이며, 이로 인해 OS는 디스크와 메모리 사이로 페이지를 이동하는 역할을 하게 된다. 하지만 이는 *mmap*이 페이지 결함(page fault)이 발생하면, 해당 프로세스가 멈추게 될 것이다.
- 쓰기 작업을 해야한다면, DBMS에 *mmap*을 사용하지 않는 것이 좋다.
- DBMS는 항상 주어진 역할 자체를 통제하도록 설계되어 있으며, 접근 중인 데이터와 처리 중인 쿼리에 대해 DBMS가 더 잘알고 있으므로 더 효율적으로 처리할 수 있다.
- 운영체제가 항상 도움이 될 수는 없다.

아래의 세 가지를 통해 OS를 사용할 수는 있다.
- *madvise*: 특정 페이지를 읽을 계획을 OS에 알려준다.
- *mlock*: OS에게 메모리 범위를 디스크로 스왑하지 않도록 설정한다..
- *msync*: OS에게 메모리 범위를 디스크로 플러시하도록 설정한다.

우리는 정확성과 성능상의 이유로 *mmap*을 DBMS에 사용하는 것을 권장하지 않는다.

시스템이 OS가 제공할 수 있는 것처럼 보이는 기능을 갖출지라도, DBMS가 이러한 절차를 실행하도록 하는 것 자체가 더 나은 제어와 성능을 제공한다.


## 4. 파일 스토리지
DBMS는 가장 기본적인 형태로 데이터베이스를 디스크에 파일로 저장한다. 파일 계층을 사용할 수도 있고 단일 파일(예: SQLite)을 사용할 수도 있다.

OS는 이 파일들의 내용에 대해 아무것도 모른다. DBMS만이 그들의 내용을 해독할 줄 안다. 왜냐하면 그것은 DBMS에 특정한 방식으로 암호화되어 있기 때문이다.

DBMS의 *스토리지 관리자*는 데이터베이스 파일 관리를 담당한다. 그것은 그 파일들을 페이지 모음으로 나타낸다. 그것은 또한 페이지에 얼마나 많은 빈 공간이 있는지 뿐만 아니라 어떤 데이터가 읽고 쓰였는지 추적한다.


## 5. 데이터베이스 페이지
DBMS는 *page*라고 하는 고정 크기의 데이터 블록으로 하나 이상의 파일에 걸쳐 데이터베이스를 정리한다. 페이지는 다른 종류의 데이터(투클, 인덱스 등)를 포함할 수 있다. 대부분의 시스템은 페이지 내에서 이러한 유형을 혼합하지 않을 것이다. 어떤 시스템에서는 그것이 자급자족할 것을 요구하게 되는데, 이것은 각 페이지를 읽는 데 필요한 모든 정보가 페이지 자체에 있다는 것을 의미한다.

각 페이지에는 고유한 식별자가 주어진다. 데이터베이스가 단일 파일인 경우 페이지 ID는 파일 오프셋일 수 있다. 대부분의 DBMS에는 페이지 ID를 파일 경로 및 오프셋에 매핑하는 리디렉션 레이어가 있다. 시스템의 상위 레벨에서 특정 페이지 번호를 요청하면 스토리지 관리자는 페이지를 찾기 위해 해당 페이지 번호를 파일과 오프셋으로 변경해야 한다.

Mos DBMS는 가변 크기 페이지를 지원하는 데 필요한 엔지니어링 오버헤드를 피하기 위해 고정 크기 페이지를 사용한다. 예를 들어, 가변 크기 페이지의 경우 페이지를 삭제하면 hte DBMS가 쉽게 새 페이지로 채울 수 없는 파일에 구멍이 생길 수 있다.

DBMS에는 세 가지 페이지의 개념이 있다.
1. 하드웨어 페이지(일반적으로 4KB)
2. OS 페이지(4KB)
3. 데이터베이스 페이지(1-16KB)

저장 장치는 하드웨어 페이지 크기의 원자 쓰기를 보장한다. 하드웨어 페이지가 4KB이면 시스템이 디스크에서 4KB를 쓰려고 할 때 4KB가 모두 기록되거나 모두 기록되지 않는다. 즉, 우리의 데이터베이스 페이지가 우리의 하드웨어 페이지보다 클 경우, DBMS는 시스템이 다운될 때 데이터베이스 페이지를 디스크로 작성하는 것을 통해 프로그램이 부분적인 방법으로 얻을 수 있기 때문에 데이터가 안전하게 작성되도록 추가 조치를 취해야 할 것이다.


## 6. 데이터베이스 힙
디스크에서 DBMS가 원하는 페이지의 위치를 찾을 수 있는 몇 가지 방법이 있으며, 힙 파일 조직이 그 방법 중 하나이다.

*히프 파일*은 튜플이 랜덤 순서로 저장되는 페이지의 순서 없는 모음입니다.

DBMS 보조 장치는 연결된 페이지 목록 또는 페이지 디렉터리를 사용하여 *page_id*가 지정된 디스크에서 페이지를 찾으십시오.

1. **라이크 리스트**: 헤더 페이지는 사용 가능한 페이지 목록과 데이터 페이지 목록에 대한 포인터를 보관한다. 그러나 DBMS가 특정 페이지를 찾는 경우, DBMS가 찾고 있는 페이지를 찾을 때까지 데이터 페이지 목록에서 순차적인 검색을 수행해야 한다.
2. **페이지 디렉토리**: DBMS는 각 페이지의 사용 가능한 공간의 양과 함께 데이터 페이지의 위치를 추적하는 특수 페이지를 관리한다.


## 7.페이지 레이아웃
모든 페이지에는 페이지 내용에 대한 메타 데이터를 기록하는 헤더가 포함된다.
- 페이지 크기.
- 체크섬.
- DBMS 버전.
- 거래 가시성
- 일부 시스템에서는 페이지를 자체적으로 포함해야 함(예: 신탁)

데이터 배치에 대한 짚맨 접근방식은 DBMS가 한 페이지에 얼마나 많은 튜플을 저장했는지 추적한 다음 새로운 튜플을 추가할 때마다 튜플을 끝까지 추가하는 것이다.

그러나 튜플을 삭제하거나 튜플에 가변 길이 속성이 있을 때 문제가 발생한다.

이것들은 (1) 슬롯화 페이지와 (2) 로그 구조화 페이지로 된 페이지로 데이터를 배치하는 두 가지 주요 접근방식이다.

**슬롯-페이지**: 페이지 맵 슬롯을 오프셋에 매핑
- 오늘날 DBMS에서 가장 일반적인 접근 방식
- 헤더는 사용한 슬롯의 개수와 마지막으로 사용한 슬롯의 시작 위치 오프셋을 추적하여 각 튜플의 시작 위치를 추적한다.
- 튜플을 추가하기 위해서는 슬롯 배열이 처음부터 끝까지 성장하고, 튜플의 데이터는 처음부터 끝까지 커진다. 슬롯 배열과 튜플 데이터가 만나면 페이지가 꽉 찬 것으로 간주된다.

**로그 구조**: 튜플을 저장하는 대신 DBMS는 로그 레코드만 저장한다.
- 데이터베이스 수정 방법을 파일에 기록 저장(삽입, 업데이트, 삭제)
- 기록을 읽기 위해 DBMS는 로그 파일을 거꾸로 스캔하고 튜플을 "복제"한다.
- 빠른 쓰기, 잠재적으로 느린 읽기.
- DBMS는 데이터를 되돌리고 업데이트할 수 없기 때문에 부록 전용 스토리지에서 잘 작동함.
- 긴 읽기를 피하기 위해 DBMS는 로그의 특정 위치로 이동할 수 있는 인덱스를 가질 수 있다. 또한 주기적으로 로그를 압축할 수 있다(Tuple이 있었다가 업데이트된 Tuple을 삽입하는 것으로만 압축할 수 있다). 컴팩션의 문제는 DBMS가 쓰기 증폭(동일한 데이터를 반복해서 다시 쓰기)으로 끝난다는 것이다.


## 8. 튜플 레이아웃
튜플은 본질적으로 바이트의 연속이다. 그러한 바이트를 속성 유형과 값으로 해석하는 것은 DBMS의 일이다.

**Tuple Header**: 튜플에 대한 메타 데이터 포함.
- DBMS의 동시 제어 프로토콜에 대한 가시성 정보(즉, 어떤 트랜잭션이 Tuple을 생성/수정했는지에 대한 정보)
- *NULL* 값에 대한 비트맵.
- DBMS가 데이터베이스의 스키마에 대한 메타 데이터를 여기에 저장할 필요가 없다는 것은 아니다.
**Tuple Data**: 속성에 대한 실제 데이터.
- 특성은 일반적으로 테이블을 만들 때 지정하는 순서대로 저장된다.
- 대부분의 DBMS는 튜플이 페이지 크기를 초과할 수 없도록 한다.
**유니크 식별자**:
- 데이터베이스의 각 튜플에는 고유 식별자가 할당된다.
- 가장 일반적인 경우: *page_id + ( 슬롯 오프셋)*
- 애플리케이션 **cannot**은(는) 이러한 ID에 의존하여 어떤 의미도 가질 수 있다.

**정규화된 Tuple Data**: 두 개의 테이블이 연관되어 있으면 DBMS가 이들을 "사전 결합"할 수 있으므로, 표와 위는 같은 페이지에 있다. 이는 DBMS가 두 개의 별도 페이지가 아닌 한 페이지에만 로드하면 되기 때문에 읽기 속도가 빨라지지만, DBMS는 각 튜플마다 더 많은 공간을 필요로 하기 때문에 업데이트 비용이 더 많이 든다.