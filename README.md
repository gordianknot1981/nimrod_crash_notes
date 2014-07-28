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

#------------------------------------------------------------
# object inherited from generic type
# SIGSEGV: Illegal storage access. (Attempt to read from nil?)
#------------------------------------------------------------
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

#-----------------------------------------------------------
# define an ojbect property of generic type
# SIGSEGV: Illegal storage access. (Attempt to read from nil?)
#------------------------------------------------------------
```


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









