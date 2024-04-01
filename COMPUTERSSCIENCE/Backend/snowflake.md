雪花算法，是**Twiter**开源的由64位整数组成分布式**ID**，性能较高，并且在单机上递增

1. 第一位 占用1**bit**，其值始终是0，没有实际作用
2. 时间戳 占用41**bit**，单位为毫秒，总共可以容纳约**69**年的时间，这里的时间戳只是相对于某个时间的增量
3. 工作机器id 占用10**bit**，其中高位5**bit**是数据中心**ID**，地位5**bit**是工作节点**ID**，最多可以容纳**1024**个节点
4. 序列号 占用12**bit**，用来记录同毫秒内产生的不同**id**。每个节点每毫秒0开始不断累加，最多可以累加4095，同一个毫秒一共可以产生4096个ID

```go
// example
package snowflake

import (
	"fmt"
	"time"

	"github.com/bwmarrin/snowflake"
)

var node *snowflake.Node

func Init(startTime string, machineID int64) (err error) {
	var st time.Time
	st, err = time.Parse("2006-01-02", startTime)
	if err != nil {
		return
	}
	snowflake.Epoch = st.UnixNano() / 1000000
	node, err = snowflake.NewNode(machineID)
	return
}

func GetID() int64 {
	return node.Generate().Int64()
}

func main() {
	if err := Init("2020-07-01", 1); err != nil {
		fmt.Printf("init failed, err:%v\n", err)
		return
	}
	id := GetID()
	fmt.Println(id)
}

```

