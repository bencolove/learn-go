# datetime

```go
const (
	dateFormat = "2006-01-02"
	timeFormat = "15:04:05"
	datetimeFormat = "2006-01-02 15:04:05"
	zoneFormat = "MST"
	offsetFormat = "2006-01-02 15:04:05:-0700"
)

func main() {
	p := log.Println
	pf := log.Printf

	date := "2020-07-13"
	t, _ := time.Parse(dateFormat, date)
	p(t.Format(dateFormat))
	
	timeValue := "11:22:33"
	t, _ = time.Parse(timeFormat, timeValue)
	p(t.Format(timeFormat)) 
	
	datetime := "2020-07-13 11:22:33"
	t, _ = time.Parse(datetimeFormat, datetime)
	p(t.Format(datetimeFormat))
	
	// timezone conversion
	chinaZone,_ := time.LoadLocation("Asia/Shanghai")
	twZone,_ := time.LoadLocation("Asia/Taipei")
	utc,_ := time.LoadLocation("UTC")
	
	// treated as UTC zoned
	naiveDatetime := "2020-07-13 11:22:33"
	t, _ = time.Parse(datetimeFormat, naiveDatetime)
	pf("string to time(as UTC) %s\n", t)
	
	// convert timezone
	cnDt := t.In(chinaZone)
	pf("converted from UTC to CN: %s\n", cnDt.Format(offsetFormat))
	
	// convert from zone-aware instance
	twDt := cnDt.In(twZone)
	pf("converted from CN  to TW: %s\n", twDt.Format(offsetFormat))
	
	
	utcDt := twDt.In(utc)
    pf("converted from TW to UTC: %s\n", utcDt)
}
```
output:  
2009/11/10 23:00:00 2020-07-13  
2009/11/10 23:00:00 11:22:33  
2009/11/10 23:00:00 2020-07-13 11:22:33  
2009/11/10 23:00:00 string to time(as UTC) 2020-07-13 11:22:33 +0000 UTC  
2009/11/10 23:00:00 converted from UTC to CN: 2020-07-13 19:22:33:+0800  
2009/11/10 23:00:00 converted from CN  to TW: 2020-07-13 19:22:33:+0800  
2009/11/10 23:00:00 converted from TW to UTC:  2020-07-13 11:22:33 +0000 UTC