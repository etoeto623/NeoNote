> neotify是一个代办任务提醒工具，是一个groovy脚本
``` groovy
#!/usr/bin/env groovy
/**
 * 系统添加crontab任务
 * crontab -e
 * 0 * * * * neotify -execute
 * 或者
 * nohup neotify -daemon >/dev/null 2>&1 &
 */
import java.text.SimpleDateFormat
import static java.lang.System.*

class Reminder{
    String content
    String time
    int notifyAheadMinute
}
def reminderToLine(Reminder reminder){
    return "${reminder.time}${SEP}${reminder.notifyAheadMinute}${SEP}${reminder.content}\n"
}
def remind(String message, deadline="10分钟以后"){
    "terminal-notifier -title 代办提醒 -subtitle $deadline -message $message -appIcon file:///${homeDir}/neotify.png".execute()
}
def parseMillsToDate(long mills){
    Calendar calendar = Calendar.getInstance()
    calendar.timeInMillis = mills
    return calendar.time
}
def getSdfDigit(){
    return new SimpleDateFormat("yyyyMMddHHmm")
}
def getSdfDay(){
    return new SimpleDateFormat("yyyyMMdd")
}
def getSdfPrint(){
    return new SimpleDateFormat("yyyy-MM-dd HH:mm")
}
def getSdfHourMin(){
    return new SimpleDateFormat("HH点mm分")
}
def getSdfHH(){
    return new SimpleDateFormat("yyyyMMddHH")
}
def getSdfHHChinese(){
    return new SimpleDateFormat("yyyy年MM月dd日 HH点")
}
def getSEP(){
    return "SEP"
}
// 获取home目录
def getHomeDir(){
    getenv("HOME")
}
def wrongArgs(){
    println "wrong argument"
    exit(1)
}
def getTaskFile(){
    return new File("${homeDir}/.neotify.task")
}
// 读取现有所有的提醒事项
List<Reminder> readReminder(){
    def file = taskFile
    if(!file.exists()){
        return new Reminder[0]
    }
    List<Reminder> reminders = new ArrayList<>()
    file.eachLine {
        String[] parts = it.split(SEP)
        Reminder rmd = new Reminder()
        rmd.time = parts[0]
        rmd.notifyAheadMinute = parts[1] as int
        rmd.content = parts[2]
        reminders.add(rmd)
    }
    return reminders
}
// 添加提醒
def addReminder(String content, String timeStamp, String ahead="10"){
    if(timeStamp.length() == 4){
        timeStamp = "${sdfDay.format(new Date())}$timeStamp"
    }
    taskFile << "${timeStamp}${SEP}${ahead}${SEP}${content}\n"
}
// 全量执行提醒
def executeTaskLoop(){
    def now = currentTimeMillis()
    def sdf = sdfDigit
    StringBuilder sb = new StringBuilder()
    boolean rewrite = false
    readReminder().each {
        def timestamp = sdf.parse(it.time).time
        if(now > timestamp){
            remind(it.content)
            rewrite = true
            sleep(500)
        }else{
            sb.append(reminderToLine(it))
        }
    }
    if(rewrite){
        taskFile.text = sb.toString()
    }
}
// 查看所有的待提醒项
def viewTasks(){
    def sdf = sdfHourMin
    def sdfOrigin = sdfDigit
    def sdfHour = sdfHH
    TreeMap<Long, List<Reminder>> remindMap = new TreeMap<>()
    readReminder().each {
        long timeHour = sdfHour.parse(it.time[0..-3]).time
        if(!remindMap.containsKey(timeHour)){
            remindMap.put(timeHour, new ArrayList<Reminder>())
        }
        remindMap.get(timeHour).add(it)
    }
    def sdfHourChinese = sdfHHChinese
    remindMap.entrySet().each {
        def date = parseMillsToDate(it.key)
        println "--------------------------------------------------"
        println "\033[35m ${sdfHourChinese.format(date)} \033[39m"
        it.value.each {
            println "\033[1;31m☞\033[0m  \033[7m${sdf.format(sdfOrigin.parse(it.time))}\033[27m " +
                    "\033[1m ${it.content} \033[22m"
        }
    }
}
// 删除提醒
def removeReminder(String time){
    if(time.length() == 4){
        time = "${sdfDay.format(new Date())}$timeStamp"
    }
    StringBuilder sb = new StringBuilder()
    readReminder().each {
        if(!it.time.equals(time)){
            sb.append(reminderToLine(it))
        }
    }
    taskFile.text = sb.toString()
}

/*********************
 *     脚本逻辑开始
 *********************/
if(null == args || args.length==0){
    println """neotify usage:
    -add yyyyMMddHHmm [aheadMinutes] content
        |- 添加reminder
    -execute
        |- 执行reminder
    -view
        |- 查看当前所有的reminder
    -remove time
        |- 根据提醒时间删除reminder
"""
    exit(1)
}
def cmd = args[0]

switch(cmd) {
    case "-add":
        // 添加提醒
        if(args.length == 3){
            addReminder(args[2],args[1])
        }else if(args.length == 4){
            addReminder(args[3],args[1],args[2])
        }else{
            wrongArgs()
        }
        break
    case "-execute":
        // 执行任务
        executeTaskLoop()
        break
    case "-daemon":
        while (true){
            executeTaskLoop()
            sleep(60000)
        }
        break
    case "-view":
        viewTasks()
        break
    case "-remove":
        if(args.length < 2){
            wrongArgs()
        }
        removeReminder(args[1])
        break
    default:
        wrongArgs()
}
```


#groovy 