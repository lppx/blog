# go 获取http请求ip


```go
package main
import (
	"encoding/json"
	"flag"
	"log"
	"net/http"
	"strings"
)
type CommandModel struct {
	Port *string
}
var commandModel CommandModel
func main() {
	initCommandModel()
	http.HandleFunc("/", ExampleHandler)
	log.Printf("running at %s...", *commandModel.Port)
	if err := http.ListenAndServe(":"+*commandModel.Port, nil); err != nil {
		panic(err)
	}

}
func initCommandModel() {
	commandModel.Port = flag.String("p", "8080", "请输入端口号")
	flag.Parse()
}
func ExampleHandler(w http.ResponseWriter, r *http.Request) {
	w.Header().Add("Content-Type", "application/json")
	resp, _ := json.Marshal(map[string]string{
		"ip": GetIP(r),
	})
	w.Write(resp)
}
// GetIP gets a requests IP address by reading off the forwarded-for
// header (for proxies) and falls back to use the remote address.
func GetIP(r *http.Request) string {
	forwarded := r.Header.Get("X-FORWARDED-FOR")
	if forwarded != "" {
		return forwarded
	}
	arr := strings.Split(r.RemoteAddr,":")
	if len(arr) > 0{
		return arr[0]
	}
	return r.RemoteAddr
}




```


