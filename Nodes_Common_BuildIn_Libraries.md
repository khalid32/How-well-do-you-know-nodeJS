# Nodes Common BuildIn Libraries

## Working with the Operating System
#### OS module
```bash
> os.
os.__defineGetter__      os.__defineSetter__      os.__lookupGetter__      os.__lookupSetter__      os.__proto__             os.constructor           os.hasOwnProperty        os.isPrototypeOf         os.propertyIsEnumerable
os.toLocaleString        os.toString              os.valueOf               

os.EOL                   os.arch                  os.constants             os.cpus                  os.endianness            os.freemem               os.getNetworkInterfaces  os.getPriority           os.homedir
os.hostname              os.loadavg               os.networkInterfaces     os.platform              os.release               os.setPriority           os.tmpDir                os.tmpdir                os.totalmem
os.type                  os.uptime                os.userInfo
```

We can read information about CPUs, like their modul, speed, and times.

<details><summary><b>os.cpus()</b></summary>
<p>

```bash
> os.cpus()
[ { model: 'Intel(R) Core(TM) i7-6500U CPU @ 2.50GHz',
    speed: 500,
    times:
     { user: 23605300,
       nice: 3500,
       sys: 7957400,
       idle: 193333100,
       irq: 0 } },
  { model: 'Intel(R) Core(TM) i7-6500U CPU @ 2.50GHz',
    speed: 500,
    times:
     { user: 23271800,
       nice: 3000,
       sys: 7959700,
       idle: 193982800,
       irq: 0 } },
  { model: 'Intel(R) Core(TM) i7-6500U CPU @ 2.50GHz',
    speed: 500,
    times:
     { user: 22512400,
       nice: 4400,
       sys: 7887700,
       idle: 178769500,
       irq: 0 } },
  { model: 'Intel(R) Core(TM) i7-6500U CPU @ 2.50GHz',
    speed: 500,
    times:
     { user: 23186300,
       nice: 3400,
       sys: 8259600,
       idle: 184621400,
       irq: 0 } } ]
```
</p>
</details>

We can read information about network interfaces. We can read their IP addresses, mac addresses, netmasks and families.

<details><summary><b>os.networkInterfaces()</b></summary>
<p>

```bash
> os.networkInterfaces()
{ lo:
   [ { address: '127.0.0.1',
       netmask: '255.0.0.0',
       family: 'IPv4',
       mac: '00:00:00:00:00:00',
       internal: true,
       cidr: '127.0.0.1/8' },
     { address: '::1',
       netmask: 'ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff',
       family: 'IPv6',
       mac: '00:00:00:00:00:00',
       scopeid: 0,
       internal: true,
       cidr: '::1/128' } ],
  wlp2s0:
   [ { address: '192.168.31.139',
       netmask: '255.255.255.0',
       family: 'IPv4',
       mac: '68:07:15:c0:ca:ba',
       internal: false,
       cidr: '192.168.31.139/24' },
     { address: 'fe80::6a97:7d4b:a217:719f',
       netmask: 'ffff:ffff:ffff:ffff::',
       family: 'IPv6',
       mac: '68:07:15:c0:ca:ba',
       scopeid: 3,
       internal: false,
       cidr: 'fe80::6a97:7d4b:a217:719f/64' } ] }
```
</p>
</details>

We can read information about total and free memory, where is the OS temp directory, and most importantly, what Operating System was Node compiled for.
```bash
> os.freemem()
1793871872
```

The type method will return Linux or Windows_NT, or Darwin for OS X. We can use that method, for example to write code specific to an Operating System.
```bash
> os.type()
'Linux'
```

We can also read the release version of the Operating System with the release method.
```bash
> os.release()
'4.15.0-39-generic'
```

The userInfo returns an object with information about the current username, uid, gid, shell and home directory. On Windows, the shell attribute is null and both uid and gid are -1.

```bash
> os.userInfo()
{ uid: 1000,
  gid: 1000,
  username: 'k32',
  homedir: '/home/k32',
  shell: '/bin/bash' }
```

os.constants return an object with all the operating system error codes and process signals.

The signal list is a handy, quick way to see a reference of all process signals available in the underlying OS.
<details><summary><b>Signal list</b></summary>
<p>

```bash
> os.constants.signals
{ SIGHUP: 1,
  SIGINT: 2,
  SIGQUIT: 3,
  SIGILL: 4,
  SIGTRAP: 5,
  SIGABRT: 6,
  SIGIOT: 6,
  SIGBUS: 7,
  SIGFPE: 8,
  SIGKILL: 9,
  SIGUSR1: 10,
  SIGSEGV: 11,
  SIGUSR2: 12,
  SIGPIPE: 13,
  SIGALRM: 14,
  SIGTERM: 15,
  SIGCHLD: 17,
  SIGSTKFLT: 16,
  SIGCONT: 18,
  SIGSTOP: 19,
  SIGTSTP: 20,
  SIGTTIN: 21,
  SIGTTOU: 22,
  SIGURG: 23,
  SIGXCPU: 24,
  SIGXFSZ: 25,
  SIGVTALRM: 26,
  SIGPROF: 27,
  SIGWINCH: 28,
  SIGIO: 29,
  SIGPOLL: 29,
  SIGPWR: 30,
  SIGSYS: 31,
  SIGUNUSED: 31 }
```

</p>
</details>

## Working with the File System
The fs module provides simple File System I/O functions to use with Node.

<details>
<summary><b>fs module</b></summary>
<p>

```bash
> fs.
fs.__defineGetter__      fs.__defineSetter__      fs.__lookupGetter__      fs.__lookupSetter__      fs.__proto__             fs.constructor           fs.hasOwnProperty        fs.isPrototypeOf         fs.propertyIsEnumerable
fs.toLocaleString        fs.toString              fs.valueOf               

fs.Dirent                fs.F_OK                  fs.FileReadStream        fs.FileWriteStream       fs.R_OK                  fs.ReadStream            fs.Stats                 fs.SyncWriteStream       fs.W_OK
fs.WriteStream           fs.X_OK                  fs._toUnixTimestamp      fs.access                fs.accessSync            fs.appendFile            fs.appendFileSync        fs.chmod                 fs.chmodSync
fs.chown                 fs.chownSync             fs.close                 fs.closeSync             fs.constants             fs.copyFile              fs.copyFileSync          fs.createReadStream      fs.createWriteStream
fs.exists                fs.existsSync            fs.fchmod                fs.fchmodSync            fs.fchown                fs.fchownSync            fs.fdatasync             fs.fdatasyncSync         fs.fstat
fs.fstatSync             fs.fsync                 fs.fsyncSync             fs.ftruncate             fs.ftruncateSync         fs.futimes               fs.futimesSync           fs.lchmod                fs.lchmodSync
fs.lchown                fs.lchownSync            fs.link                  fs.linkSync              fs.lstat                 fs.lstatSync             fs.mkdir                 fs.mkdirSync             fs.mkdtemp
fs.mkdtempSync           fs.open                  fs.openSync              fs.promises              fs.read                  fs.readFile              fs.readFileSync          fs.readSync              fs.readdir
fs.readdirSync           fs.readlink              fs.readlinkSync          fs.realpath              fs.realpathSync          fs.rename                fs.renameSync            fs.rmdir                 fs.rmdirSync
fs.stat                  fs.statSync              fs.symlink               fs.symlinkSync           fs.truncate              fs.truncateSync          fs.unlink                fs.unlinkSync            fs.unwatchFile
fs.utimes                fs.utimesSync            fs.watch                 fs.watchFile             fs.write                 fs.writeFile             fs.writeFileSync         fs.writeSync
```

</p>
</details>

All of the fs module functions have asynchronous and synchronous forms. You can pick either form depending on your code logic.
For example, if you're reading a file during a server initialization process, `readFileSync` is probably ok, but if you're reading a file every time a user requests something from that server, you should probably stick with the asynchronous form.

Other than being synchronous or asynchronous, those forms handle exceptions differently. The async functions pass any encountered errors normally as the first argument in the callback, while the synchronous functions immediately throw any errors.
When using the synchronous functions, if you don't want the errors to bubble up, you'll need to use a try/catch statement to handle them.

## Console and Utilities
The console module is designed to match the console object provided by web browsers.
```bash
> console
Console {
  log: [Function: bound consoleCall],
  debug: [Function: bound consoleCall],
  info: [Function: bound consoleCall], ## <-- is just an alias to `console.log`
  dirxml: [Function: bound consoleCall],
  warn: [Function: bound consoleCall], ## <-- is an alias for `console.error`
  error: [Function: bound consoleCall], ## <-- behaves exactly like console.log, but writes to `stderr` instead of `stdout`...
  dir: [Function: bound consoleCall],
  =======================================|
  time: [Function: bound consoleCall],   |
  timeEnd: [Function: bound consoleCall],|
  ======================================== ## <-- to start and stop timers and report the duration of an operation.
  timeLog: [Function: bound timeLog],
  trace: [Function: bound consoleCall], ## <-- behaves jsut like `console.error`, but it also prints the call stack at the point where it is placed, which is handy when debugging problems.
  assert: [Function: bound consoleCall],
  clear: [Function: bound consoleCall],
  count: [Function: bound consoleCall],
  countReset: [Function: bound consoleCall],
  group: [Function: bound consoleCall],
  groupCollapsed: [Function: bound consoleCall],
  groupEnd: [Function: bound consoleCall],
  table: [Function: bound consoleCall],
  Console: [Function: Console],
  markTimeline: [Function: markTimeline],
  profile: [Function: profile],
  profileEnd: [Function: profileEnd],
  timeline: [Function: timeline],
  timelineEnd: [Function: timelineEnd],
  timeStamp: [Function: timeStamp],
  context: [Function: context],
  [Symbol(counts)]: Map {},
  [Symbol(kColorMode)]: 'auto' }
```

In Node.js, there is a Console class that we can use to write to any Node.js stream, and there is a global console object already confirmed to write to `stdout` and `stderr`. Those are 2 different things.
```bash
> console.Console
[Function: Console]
```

Console.log uses the `util` module under the hood to format and output a message with a new line.
```bash
> util.
util.__defineGetter__        util.__defineSetter__        util.__lookupGetter__        util.__lookupSetter__        util.__proto__               util.constructor             util.hasOwnProperty          util.isPrototypeOf
util.propertyIsEnumerable    util.toLocaleString          util.toString                util.valueOf                 

util.TextDecoder             util.TextEncoder             util._errnoException         util._exceptionWithHostPort  util._extend                 util.callbackify             util.debug                   util.debuglog
util.deprecate               util.error                   util.format                  util.formatWithOptions       util.getSystemErrorName      util.inherits                util.inspect                 util.isArray
util.isBoolean               util.isBuffer                util.isDate                  util.isDeepStrictEqual       util.isError                 util.isFunction              util.isNull                  util.isNullOrUndefined
util.isNumber                util.isObject                util.isPrimitive             util.isRegExp                util.isString                util.isSymbol                util.isUndefined             util.log
util.print                   util.promisify               util.puts                    util.types
```
