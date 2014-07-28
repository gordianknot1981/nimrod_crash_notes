Illegal storage access
==================
```
type
   Super[S] = ref object of TObject
      name:string
      mode:string
      data:seq[S]

type 
   Sub[S] = ref object of Super[S]
      p_state: S

var sb:Sub[string] = Sub[string](name: "sub")
```

SIGSEGV: Illegal storage access. (Attempt to read from nil?)
Nimrod ver.  0.9.4 (2014-04-21)

this works.... 
```
type
   Super[S] = ref object of TObject
      name:string
      mode:string
      data:seq[S]

type 
   Sub[S] = ref object of Super[string]    # change from generic [S] to [string]
      p_state: S

var sb:Sub[string] = Sub[string](name: "sub")
```


```
type
   KProperty[S] = ref object of TObject
      p_current: S
 
   TProperties = object
      propCint: KProperty[cint]
     

type Widget* = ref object of TObject
   p: KProperty[cint]

proc proneError(self:var Widget) = discard
```
shows
SIGSEGV: Illegal storage access. (Attempt to read from nil?)

But this works
```
type
   KProperty[S] = ref object of TObject
      p_current: S
type
   TProperties = object
      propCint: KProperty[cint]
     

type Widget* = ref object of TObject
   p: KProperty[cint]

proc proneError(self:var Widget) = discard
```




