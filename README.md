note: Illegal storage access
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


to clear the problem
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

note: macro bug
=======================
```
tmp = iintTable[string,string]()
tmp["properties"] = "width"
tmp["properties"] = "height"
for property, typ in tmp.pairs():
         block:
            let vb = property
            let mvb = "p_" + vb
            let fetch1 = "fetch_" + property + "_getter"
            let fetch2 = "fetch_" + property + "_setter"

#-------------------------------------------------------
# in some circumstances nimrod is prone bug by messing
# up reistered value when using macros. In above case,
# instead of "fetch_width_getter", or "fetch_height_getter", 
# "fetch" was messed up by some odd value like "propertieswidth_getter"
# or "propertiesheight_getter
#
# to clear this...


      let fe = "fetch_"
      let se = "_setter"
      let ge = "_getter"
      for property, typ in tmp.pairs():
         block:
            let vb = property
            let mvb = "p_" + vb
            let fetch1 = fe + property + ge
            let fetch2 = fe + property + se

```

















