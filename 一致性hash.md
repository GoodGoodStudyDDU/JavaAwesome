由 $n = 2^{32}$个桶组成的环
每个节点占一个桶，桶的选择为 $pos = key\%n$，其中key为桶的唯一标识。
数据采用同样的方式进行hash，并在环上以顺时针方向找节点，找到第一个为止，此时该数据存放在该节点中
如果节点数量较少且很不均匀，可以采用虚拟节点，每个节点映射若干个虚拟节点，并且在环上hash


一致性hash的好处：
传统的取模hash，当节点数量变化时，会导致所有数据再hash的过程。在一致性hash中，如果节点数量发生了变化，仅需要调整部分数据。