Skip List는 AVL / Red-Black 트리와 같은 로그 시간만큼의 효율성을 가진 확률적인 데이터 구조이며 검색 및 업데이트 작업을 효율적으로 지원하며  
다른 데이터 구조와 비교하여 구현하기가 비교적 간단합니다.

Skip List S는 일련의 정렬 된 연결리스트{L0, ..., Ln}, 계층화 된 Layer로 구성되며 각 계층 L은 계층 L0에있는 항목의 하위 집합을 증분 순서로 저장합니다. 계층 {L1, ..., Ln}의 항목은 확률 1/2로 동전 뒤집기 기능을 기반으로 무작위로 선택됩니다. 순회의 경우 계층의 모든 항목은 아래 노드와 다음 노드를 참조 합니다. 이 계층은 그 아래의 계층에 대해 고속 차선 역할을하므로 레인을 건너뛰고 이동 거리를 줄이고 정기적 인 링크 목록에서 예상대로 O (n)을 검색하는 등 빠른 O (log n) 검색이 가능합니다.

Skip List S :

  1. 리스트 L0에는 삽입 된 모든 item이 포함됩니다.
  2. 리스트 {L1, ..., Ln}에 대하여, Li는 Li-1에서 임의로 생성 된 리스트의 부분 집합을 포함합니다.
  3. 높이는 동전 뒤집기로 결정됩니다

![Schematic view](Images/Intro.png)
Figure 1


#Searching

N개의 element 검색은 최상위 계층 Ln에서 L0까지 횡단하여 시작합니다.

우리의 목적은 현재 레이어의 가장 오른쪽 위치에있는 값이 목표 아이템보다 작고 후속 노드가 더 큰 값 또는 0을 갖도록 요소 K를 찾는 것입니다 (K.key <N.key <= (K .next.key 또는 nil)). K.next의 값이 N과 같으면 검색을 종료하고 K.next를 반환하고, 그렇지 않으면 K.down을 사용하여 아래 노드 (계층 Ln-1)로 떨어 뜨리고 K.down이 L0이 될 때까지 프로세스를 반복합니다. 레벨이 L0이고 항목이 존재하지 않을 때는 nil로 나타냅니다.


###Example:

![Inserting first element](Images/Search1.png)

#Inserting

element N을 삽입하는 것은 search와 유사한 프로세스를가집니다. 최상위 레이어 Ln에서 L0까지 순회하는 것으로 시작합니다. 스택을 사용하여 경로를 추적해야합니다. 동전 뒤집기가 시작될 때 경로를 위쪽으로 탐색하는 데 도움이되므로 새 element를 삽입하고 참조를 업데이트 할 수 있습니다.

우리의 목적은 레이어 Ln의 가장 오른쪽 위치에있는 값이 새로운 아이템보다 작고 후속 노드가 더 큰 값 또는 nil을 갖는 element K를 찾는 것입니다 (K.key <N.key <(K. next.key 또는 nil)). 요소 K를 스택 및 element K로 밀어 넣고 K.down을 사용하여 아래 노드 (계층 Ln-1)로 이동 한 다음 L.o까지 프로세스를 반복합니다 (여기서 K.down은 레벨이 없음을 나타내는 nil입니다). L0. K.down이 0 일 때 프로세스를 종료합니다.

L0에서 K 다음에 N을 삽입 할 수 있습니다.

여기 재미있는 부분이 있습니다. 무작위로 레이어를 만들기 위해 동전 뒤집기 기능을 사용합니다.

코인 flip 함수가 0을 반환하면 전체 프로세스가 완료되지만 1을 반환하면 두 가지 가능성이 있습니다.

스택이 비어 있습니다 (레벨은 L0 / - Ln 또는 초기화되지 않은 스테이지에서 입니다).
스택에 항목이 있습니다 (위로 이동 가능).


In case 1 :

새로운 계층 M *은 헤드 노드 NM이 아래의 계층의 헤드 노드를 참조하고 NM.next가 새로운 요소 N을 참조하여 생성됩니다. 새로운 요소 N 이전 계층에서 요소 N을 참조합니다.

In case 2 :

스택이 비어있을 때까지 반복 스택에서 항목 F를 popping하고 이에 따라 참조를 업데이트하십시오. F.next는 K.next이고 K.next는 F가 될 것입니다.

스택이 비어있을 때 아래 레이어의 헤드 노드를 참조하는 헤드 노드 NM과 새 요소 N을 참조하는 NM.next를 참조하여 새 레이어 consisintg을 만듭니다. 새 요소 N 이전 레이어에서 요소 N을 참조합니다.
		 

###Example:

Inserting 13. with coin flips (0)

![Inserting first element](Images/Insert5.png)
![Inserting first element](Images/Insert6.png)
![Inserting first element](Images/insert7.png)
![Inserting first element](Images/Insert8.png)
![Inserting first element](Images/Insert9.png)



Inserting 20. with 4 times coin flips (1) 
![Inserting first element](Images/Insert9.png)
![Inserting first element](Images/Insert10.png)
![Inserting first element](Images/Insert11.png)
![Inserting first element](Images/Insert12.png)

#Removing

Removing은 insert 과정과 유사합니다.
TODO

#더 궁금한 점이 있다면, 

[Skip List on Wikipedia](https://en.wikipedia.org/wiki/Skip_list) 

Written for Swift Algorithm Club by [Mike Taghavi](https://github.com/mitghi)
