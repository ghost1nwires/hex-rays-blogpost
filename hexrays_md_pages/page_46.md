# Hex-Rays Blog – Page 46

**Archived:** 2026-02-20 12:24
**URL:** https://hex-rays.com/blog/page/46

---

## 1. 

**Date:** Posted: Nov 20, 2009  
**Link:** [https://hex-rays.com/blog/hex-rays-plugin-contest](https://hex-rays.com/blog/hex-rays-plugin-contest)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-Rays Plugin Contest

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-plugin-contest>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-plugin-contest>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-plugin-contest>)

Ilfak Guilfanov ✦ Posted: Nov 20, 2009

![Hex-Rays Plugin Contest](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-07-48-3471-AM.png)

We are glad to announce the results of our first plugin contest! For the contest rules, please check this page:  
[http://www.hex-rays.com/contests/](<http://www.hex-rays.com/contests>)  
Or you may directly go to the contest results and check out some cool plugins:  
<https://www.hex-rays.com/contests_details/contest2009/>  
It was our first contest, but we are happy with the results and will repeat it in the near future.  
Have fun!

---

## 2. 

**Date:** Posted: Oct 21, 2009  
**Link:** [https://hex-rays.com/blog/hex-rays-is-hiring](https://hex-rays.com/blog/hex-rays-is-hiring)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-Rays is hiring

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-is-hiring>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-is-hiring>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-is-hiring>)

Ilfak Guilfanov ✦ Posted: Oct 21, 2009

![Hex-Rays is hiring](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

We are looking for someone to join our team and participate in the development of unique software security tools. The candidates must know low-level details of modern software as well as high-level data structures and algorithms.  
Requirements:  
* strong knowledge of C/C++  
* **experience with Qt and GUI development is a big PLUS**  
* knowledge of x86 assembler and unwillingness to use it in development  
* cross platform development (Windows/Linux/Mac) is a plus  
* knowing the graph theory and how compilers work is a plus  
* ability and willingness to write secure yet fast code  
* good problem solving and communication skills  
To apply, please send your resume to info@hex-rays.com  
Code samples and links to implemented projects are welcome.

---

## 3. 

**Date:** Posted: Oct 15, 2009  
**Link:** [https://hex-rays.com/blog/hex-rays-decompiler-primer](https://hex-rays.com/blog/hex-rays-decompiler-primer)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Hex-Rays Decompiler primer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/hex-rays-decompiler-primer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/hex-rays-decompiler-primer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/hex-rays-decompiler-primer>)

Elias Bachaalany ✦ Posted: Oct 15, 2009

![Hex-Rays Decompiler primer](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

The [Hex-Rays Decompiler](<http://www.hex-rays.com/products/decompiler/>) 1.0 was released more than two years ago.  
Since then it has improved a lot and does a great job decompiling real-life code, but sometimes there are additional things that you might wish to do with its output.  
For that purpose we have released the Hex-Rays [Decompiler SDK](</blog/hexrays-sdk-is-ready>) and several sample plugins.  
However, the header files alone do not give a complete picture and it can be difficult to see where to start.

In this post we will outline the architecture of the Hex-Rays Decompiler SDK, cover some principles and finally wrap everything we discussed and write a small plugin.  
  
The post is divided into the following sections:

  * Background information
  * The citem_t class
  * The cexpr_t, cinsn_t and their subclasses
  * The cfunc_t class
  * The tree visitor class
  * A sample plugin



## Background information

If you’re already familiar with abstract syntax trees (aka ASTs), you can skip this section. Otherwise, some basic information is available at [Wikipedia](<http://en.wikipedia.org/wiki/Abstract_syntax_tree>). It can also be useful to read Ilfak’s post “[Decompiler output ctree](</blog/decompiler-output-ctree>)“.

We will provide some examples to show how ASTs are used in the decompiler. Consider this C code:
    
    
    int func(int a)
    {
      int result;
      if ( a == 1 )
      {
        result = 5;
      }
      else
      {
        result = 6;
      }
      return result;
    }

It can be represented with this AST:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hri_1-thumb-3.jpg?width=400&height=359&name=hri_1-thumb-3.jpg)](</ida_pro/pix/hri_1.html>)

From the start node, we see a “{2}” node that denotes a block of two statements. The first statement in the block is an _if_ and the second statement is a _return_.  
If we take the first statement (_if_) we notice that it has 3 nodes attached to it: (1) the _condition_ node (2) the _then-branch_ node and (3) the _else-branch_ node (optional).  
Looking further down in the tree we notice that the condition node is an _equals to_ node which also links to two other expression nodes _x_ and _y_.  
If we take the _then-branch_ we see how the _result = 5_ got translated into an assignment node _x = y_ with the _x_ node being a variable and the _y_ node being a numeric constant.

Let us take another code snippet:
    
    
    int func1(int n)
    {
      return (n | 291) & 4011;
    }

And see its AST:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hri_2-thumb-3.jpg?width=400&height=619&name=hri_2-thumb-3.jpg)](</ida_pro/pix/hri_2.html>)

Again we have a block containing one statement: the _return_ statement. The return statement has an expression of type binary AND, and this expression takes two operands _x_ and _y_. The _x_ operand is another expression (binary OR) and the _y_ operand is a numeric constant (value = 291).  


## The citem_t class

Hex-Rays decompiler SDK refers to the AST as _ctree_ and each node within the tree is represented by either a _cinsn_t_ or a _cexpr_t_ class instance. Both those classes are descendants of the _citem_t_ base class:
    
    
    struct citem_t
    {
      ea_t ea; // address that corresponds to the item
      ctype_t op;// element type
      ...
    }
    

One of the most important fields in a _citem_t_ is its _op_ field value which is of type _ctype_t_. _ctype_t_ is an enum with constants identifying the type of the node.  
Hex-Rays defines two types of constants: _cot_xxxx_ and _cit_xxxx_. The former denote expression items while the latter denote statements (or instructions in Hex-Rays jargon).

Let us take a look at some of the ctype_t constants:
    
    
    enum ctype_t
    {
      cot_empty    = 0,
      cot_asg      = 2,   //  x = y
      cot_bor      = 19,  //  x | y (binary OR)
      cot_band     = 21,  //  x & y (binary AND)
      cot_eq       = 22,  //  x == y
      cot_add      = 35,  //  x + y
      cot_call     = 57,  //  x(...)
      cot_num      = 61,  //  n
      cot_fnum     = 62,  //  fpc
      ...
      cot_last     = cot_helper,
      // The statements
      cit_block    = 70,  //  block-statement: { ... }
      cit_if       = 72,  //  if-statement
      cit_return   = 79,  //  return-statement
      ...
    }
    

Now given a _citem_t_ instance we can check its _op_ value and see whether to treat that citem_t as a _cexpr_t_ or a _cinsn_t_.  


## The cexpr_t class

Let us illustrate how a _cexpr_t_ class instance can represent items with _cot_xxxx_ type values. Below we outline the class members that are relevant to our discussion:
    
    
    // Ctree element: expression.
    // Depending on the exact expression item type,
    // various fields of this structure are used.
    struct cexpr_t : public citem_t
    {
      ...
      union
      {
        //  used for cot_num
        cnumber_t * **n** ;
        //  used for cot_fnum
        fnumber_t *fpc;
        struct
        {
          union
          {
            var_ref_t v;  //  used for cot_var
            ea_t obj_ea;  //  used for cot_obj
          };
          //  how many bytes are accessed? (-1: none)
          int refwidth;
        };
        struct
        {
          //  the first operand of the expression
          cexpr_t * **x** ;
          union
          {
            //  the second operand of the expression
            cexpr_t * **y** ;
            //  argument list (used for cot_call)
            carglist_t * **a** ;
            //  member offset (used for cot_memptr, cot_memref)
            uint32 m;
          };
          union
          {
            //  the third operand of the expression
            cexpr_t *z;
            //  memory access size (used for cot_ptr, cot_memptr)
            int ptrsize;
          };
        };
        //  an embedded statement, they are
        //  prohibited at the final maturity stage
        cinsn_t *insn;
        //  helper name (used for cot_helper)
        //  string constant (used for cot_str)
        char *helper;
        char *string;
      ...
      };
      ...
    };
    

As you notice, _cexpr_t_ employs unions, thus the contained information depends on the _op_ field.  
For example if _cexpr.op == cot_num_ then we can safely access _cexpr.n_ field to get a _cnumber_t_ instance and extract the constant number.  
If the expression has two operands (e.g. _cot_add_ , _cot_sub_ , _cot_bor_ and so on….), then we have two sub-expressions: _cexpr.x_ is the left-hand side operand and _cexpr.y_ is the right-hand side operand.  
In the case of a function call (denoted by _op == cot_call_) the address of the called function is accessible via _cexpr.x.obj_ea_ field and the arguments are in the _a_ field (which is a _carglist_t_ instance).

Bottom line: first check the _op_ value and then extract the fields from a _cexpr_t_ instance accordingly.  


## The cinsn_t class

This class represents statements supported by Hex-Rays (_cit_for_ , _cit_if_ , _cit_return_ , etc…). An excerpt from the class definition:
    
    
    // Ctree element: statement.
    // Depending on the exact statement type,
    // various fields of the union are used.
    struct cinsn_t : public citem_t
    {
      ...
      union
      {
        //  details of block-statement
        cblock_t * **cblock** ;
        //  details of expression-statement
        cexpr_t *cexpr;
        //  details of if-statement
        cif_t * **cif** ;
        //  details of for-statement
        cfor_t *cfor;
        //  details of while-statement
        cwhile_t *cwhile;
        //  details of do-statement
        cdo_t *cdo;
        //  details of switch-statement
        cswitch_t *cswitch;
        //  details of return-statement
        creturn_t * **creturn** ;
        //  details of goto-statement
        cgoto_t *cgoto;
        //  details of asm-statement
        casm_t *casm;
      };
      ...
    };
    

Just as we could tell what kind of _cexpr_t_ we have by looking at the _op_ field, here we check the _op_ field against the _cit_xxxx_ constants and extract data accordingly.  
For example, if _op == cit_if_ , we can extract a _cif_t_ instance from the _cif_ field. Similarly for _op == cit_return_ the corresponding field is _creturn_.

The class _cblock_t_ is used to describe a sequence of statements. It is defined as a list of _cinsn_t_ :
    
    
    // Compound statement (curly braces)
    // we need list to be able to manipulate
    // its elements freely
    struct cblock_t : public qlist<cinsn_t>
    {
      ...
      iterator find(const cinsn_t *insn);
    };
    

When Hex-Rays creates a _ctree_ , the root of the tree is a _cblock_t_ that contains all the subsequent instructions (or expressions) present in the decompiled function.  


## The ceinsn_t class

_ceinsn_t_ is used whenever we need to describe a statement that contains an expression. For example, “x = 1;” is a statement containing expression “x = 1”.
    
    
    // Statement with an expression.
    // This is a base class for various statements
    // with expressions.
    struct ceinsn_t
    {
      //  Expression of the statement
      cexpr_t expr;
    };

The _if_ statement is a statement with an expression where the expression is the condition of the _if_ :
    
    
    // If statement
    struct cif_t : public ceinsn_t
    {
      ...
      //  Then-branch of the if-statement
      cinsn_t * **ithen** ;
      //  Else-branch of the if-statement. May be NULL.
      cinsn_t * **ielse** ;
      ...
    };
    

Given a _cif_t_ instance, we can extract its _condition_ by accessing _cif_t.expr_ field, the _then-branch_ from _cif_t.ithen_ (a _cinsn_t_ , which in turn could be a _cblock_t_ holding many other _cinsn_t_ instances), and the _else-branch_ can be accessed through _cif_t.ielse_ , if present.

Before illustrating other statements with expressions (such as the _for_ statement), let us talk about the _cloop_t_ class which is used to represent repetition structures: for, while, do.
    
    
    // Base class for loop statements
    struct cloop_t : public ceinsn_t
    {
      cinsn_t * **body** ;
      ...
    };
    

As you notice, _cloop_t_ is _ceinsn_t_ (a statement with expression) with an instruction (the _body_ member).  
The _cloop_t_ class (as is) can be used to define a do/while statement where the _expr_ field is the while’s condition and the _body_ field is the body of the do/while statement:
    
    
    // Do-loop
    struct cdo_t : public cloop_t
    {
      DECLARE_COMPARISONS(cdo_t);
    };
    

A _for_ loop statement has four components: (1) initialization expression, (2) condition expression, (3) step expression and (4) the body:
    
    
    // For-loop
    struct cfor_t : public cloop_t
    {
      cexpr_t **init** ;   //  Initialization expression
      cexpr_t **step** ;   //  Step expression
      ...
    };
    

The initialization expression is stored in the _init_ field, the condition in the base class’ _expr_ field, the step expression in the _step_ field and the body of the loop in the _body_ field.  


## The cfunc_t class

Now that we covered the basic tree elements, let us talk about the _cfunc_t_ class, which is used to hold a decompiled function:
    
    
    // Decompiled function. Decompilation result is kept here.
    struct cfunc_t
    {
      //  function entry address
      ea_t **entry_ea** ;
      //  function body, must be a block
      cinsn_t **body** ;
      //  maturity level
      ctree_maturity_t maturity;
      // The following maps must be accessed
      // using helper functions.
      // Example: for user_labels_t,
      // see functions starting with "user_labels_".
      //  user-defined labels.
      user_labels_t *user_labels;
      //  user-defined comments.
      user_cmts_t *user_cmts;
      //  user-defined number formats.
      user_numforms_t *numforms;
      //  user-defined item flags
      user_iflags_t *user_iflags;
      ...
    }
    

When Hex-Rays is asked to decompile a function it returns a _cfunc_t_ instance. The following is an excerpt from the example #1 found at the [examples page](<http://www.hex-rays.com/manual/sdk/examples.html>):
    
    
      func_t *pfn = get_func(get_screen_ea());
      if ( pfn == NULL )
      {
        warning("Please position the cursor within a function");
        return;
      }
      hexrays_failure_t hf;
      cfunc_t * **cfunc** = **decompile**(pfn, &hf);
      if ( cfunc == NULL )
      {
        warning("#error \"%a: %s", hf.errea, hf.desc().c_str());
        return;
      }
      msg("%a: successfully decompiled\n", pfn->startEA);
      qstring bodytext;
      qstring_printer_t sp(cfunc, bodytext, false);
      cfunc->print_func(sp);
      msg("%s\n", bodytext.c_str());
      delete cfunc;
    

Among the fields in _cfunc_t_ , _body_ is the most important to us because it points to the root of the _ctree_. It can be used to traverse the tree manually, but the visitor utility classes provided by the Hex-Rays SDK make that task simpler.  


## The tree visitor class

The tree visitor class is a utility class that can be used to traverse the tree, find _ctree_ items, and (if desired) modify the tree along the way.
    
    
    // A generic helper class that is used for ctree traversal
    struct ctree_visitor_t
    {
      ...
      // Traverse ctree.
      int hexapi **apply_to**(citem_t *item, citem_t *parent);
      // Visit a statement.
      virtual int idaapi **visit_insn**(cinsn_t *) { return 0; }
      // Visit an expression.
      virtual int idaapi **visit_expr**(cexpr_t *) { return 0; }
      ...
    };
    

Hex-Rays provides other visitors as well: _ctree_parentee_t_ , _user_lvar_visitor_t_ , …

To use the class, inherit from _ctree_visitor_t_ and override virtual methods as necessary. For example, here’s how to use it to report all called functions:
    
    
    void traverse(cfunc_t *cfunc)
    {
      struct sample_visitor_t : public ctree_visitor_t
      {
      public:
        sample_visitor_t() : ctree_visitor_t(CV_FAST) { }
        int idaapi visit_expr(cexpr_t *expr)
        {
          if ( expr->op != cot_call )
            return 0;
          char buf[MAXSTR];
          if (get_func_name(expr->x->obj_ea, buf, sizeof(buf)) == NULL)
            qsnprintf(buf, sizeof(buf), "sub_%a", expr->x->obj_ea);
          msg("%a: a call to %s with %d argument(s)\n", expr->ea, buf, expr->a->size());
          return 0; // continue enumeration
        }
      };
      sample_visitor_t tm;
      tm.apply_to(&cfunc->body, NULL);
    }
    

The _traverse()_ function can be called manually with a _cfunc_t *_ obtained from a decompiled function, or automatically by installing a callback:
    
    
    // This callback handles various Hex-Rays events.
    static int idaapi callback(void *, hexrays_event_t event, va_list va)
    {
      switch ( event )
      {
      case hxe_maturity:
        {
          cfunc_t *cfunc = va_arg(va, cfunc_t *);
          ctree_maturity_t new_maturity = va_argi(va, ctree_maturity_t);
          if ( new_maturity == CMAT_FINAL ) // ctree is ready
          {
            **traverse**(cfunc);
          }
        }
        break;
      }
      return 0;
    }
    int idaapi init(void)
    {
      ...
      **install_hexrays_callback**(callback, NULL);
      ...
    }
    

## Writing a plugin

Now that we covered the basics, let us put our knowledge into practice and write a small plugin. Consider this C code:
    
    
    int func3(int n, char *s)
    {
      int b;
      if ( strcmp(s, "hello") == 0 )
      {
        b = 100;
      }
      else if ( strcmp(s, "hello1") == 0 )
      {
        b = 200;
      }
      else if ( strcmp(s, "hello2") == 0 )
      {
        b = 4;
      }
      else if ( strcmp(s, "hello3") == 0 )
      {
        b = 300;
      }
      else if ( strcmp("hello4", s) == 0 )
      {
        b = 400;
      }
      else if ( strcmp("hello5", s) == 6 )
      {
        b = 500;
      }
      else
      {
        b = 600;
      }
      return b + n;
    }
    

If we compile it and decompile back with Hex-Rays decompiler, we get:
    
    
    int __cdecl func3(int n, char *s)
    {
      signed int v2; // eax@2
      if ( strcmp(s, "hello") )
      {
        if ( strcmp(s, "hello1") )
        {
          if ( strcmp(s, "hello2") )
          {
            if ( strcmp(s, "hello3") )
            {
              if ( strcmp("hello4", s) )
              {
                if ( strcmp("hello5", s) == 6 )
                  v2 = 500;
                else
                  v2 = 600;
              }
              else
              {
                v2 = 400;
              }
            }
            else
            {
              v2 = 300;
            }
          }
          else
          {
            v2 = 4;
          }
        }
        else
        {
          v2 = 200;
        }
      }
      else
      {
        v2 = 100;
      }
      return n + v2;
    }
    

With the following AST:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hri_3-thumb-3.jpg?width=400&height=641&name=hri_3-thumb-3.jpg)](</ida_pro/pix/hri_3.html>)

No doubt that Hex-Rays did an excellent job and the decompiled result is equivalent to the original function. Nonetheless, we can write a small plugin to automatically replace _if (strcmp())_ with _if (strcmp() == 0)_ and swap the then/else branches:
    
    
    if ( strcmp(a, b) )
    {
      if ( strcmp(a, c) )
      {
        // something if a != c
      }
      else
      {
        // something if a == c
      }
    }
    else
    {
      // something if a == b
    }
    

Becomes:
    
    
    if ( strcmp(a, b) == 0 )
    {
      // something if a == b
    }
    else
    {
      if ( strcmp(a, c) == 0 )
      {
        // something if a == c
      }
      else
      {
        // something if a != c
      }
    }
    

To find such pattern, we need to match _if_ statements with the following conditions:

  * The _if_ statement should have an _else_
  * The _if_ condition should be a function call to strcmp(a, b) 



After we find such a statement, we need to replace its condition expression (which is a _cot_call_) with another expression (_cot_eq_), where the first operand (_x_) is the original condition and the second operand (_y_) is the number zero. Essentially we modify the tree from:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hri_b4-thumb-3.gif?width=400&height=232&name=hri_b4-thumb-3.gif)](</ida_pro/pix/hri_b41.html>)

To:

[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/hri_a5-thumb-3.gif?width=400&height=185&name=hri_a5-thumb-3.gif)](</ida_pro/pix/hri_a51.html>)

Notice how the modified tree has the _if_ condition changed from a call to strcmp() to an expression x == y (where y is the number zero).

The code to do that should be easy to understand now, especially that we explained all the logic behind it:
    
    
    struct strcmp_inverter_t : public ctree_visitor_t
    {
    private:
      cfunc_t *cfunc;
    public:
      strcmp_inverter_t(cfunc_t *cf) : ctree_visitor_t(CV_FAST), cfunc(cf) { }
      bool is_strcmp_expr(cexpr_t *expr)
      {
        // the expression should be a function call
        if ( expr->op != cot_call )
          return false;
        // should have two arguments
        carglist_t &a = *expr->a;
        if (a.size() != 2)
          return false;
        // should contain the string str[i]cmp
        char buf[MAXSTR];
        if ( get_func_name(expr->x->obj_ea, buf, sizeof(buf)) == NULL )
          return false;
        if ( stristr(buf, "strcmp") == NULL &&
            stristr(buf, "stricmp") == NULL )
          return false;
        return true;
      }
      int idaapi visit_insn(cinsn_t *ins)
      {
        // only interested in IF statements
        if ( ins->op != cit_if )
          return 0;
        // now take the instance
        cif_t *cif = ins->cif;
        // must have an ELSE
        if ( cif->ielse == NULL )
          return 0;
        // check if it's an strcmp()
        if ( !is_strcmp_expr(&cif->expr) )
          return 0;
        // create a zero expression
        cexpr_t *y = new cexpr_t();
        y->put_number(cfunc, 0, inf.cc.size_i);
        // create a new empty expression
        cexpr_t *x = new cexpr_t();
        // now the if's expr (condition) is moved to this new condition
        x->swap(cif->expr);
        // now that the if's expr is an empty expression,
        // let us properly populate it
        cif->expr.ea = x->ea;
        cif->expr.op = cot_eq;
        cif->expr.x = x;
        cif->expr.y = y;
        cif->expr.calc_type(false);
        // we changed the condition, so we
        // should swap the THEN/ELSE branches too!
        qswap(cif->ithen, cif->ielse);
        return 0; // continue enumeration
      }
    };

Now if we use the plugin on the previously decompiled function, we get this result:
    
    
    int __cdecl func3(int n, char *s)
    {
      signed int v2; // eax@2
      if ( strcmp(s, "hello") == 0 )
      {
        v2 = 100;
      }
      else
      {
        if ( strcmp(s, "hello1") == 0 )
        {
          v2 = 200;
        }
        else
        {
          if ( strcmp(s, "hello2") == 0 )
          {
            v2 = 4;
          }
          else
          {
            if ( strcmp(s, "hello3") == 0 )
            {
              v2 = 300;
            }
            else
            {
              if ( strcmp("hello4", s) == 0 )
              {
                v2 = 400;
              }
              else
              {
                if ( strcmp("hello5", s) == 6 )
                  v2 = 500;
                else
                  v2 = 600;
              }
            }
          }
        }
      }
      return n + v2;
    }
    

Looks much closer to the original.  


## Closing words

The source code of the plugin can be downloaded from [here](</ida_pro/files/vds8.cpp>). To use it, simply right-click anywhere in a decompiled function and select “Enable auto if (strcmp()) inversion.

Last but not least, we would like to remind you that the [plugin contest](<https://www.hex-rays.com/contests/>) deadline is just one month away. If you have nice ideas, participate and show us your creativity!

---

## 4. 

**Date:** Posted: Oct 5, 2009  
**Link:** [https://hex-rays.com/blog/seh-graph](https://hex-rays.com/blog/seh-graph)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# SEH Graph

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/seh-graph>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/seh-graph>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/seh-graph>)

Elias Bachaalany ✦ Posted: Oct 5, 2009

![SEH Graph](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

It is [said](<http://en.wikipedia.org/wiki/A_picture_is_worth_a_thousand_words>) that a picture is worth a thousand words, and similarly many reversers would agree that a graph is worth a thousand lists! 😉  


Recently, we added graphing support into IDAPython and now Python scripts can build interactive graphs.  
To demonstrate this new addition, we will write a small script that graphs the structured exception handlers of a given process.

  
![sehgraph_small.png](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sehgraph_small-3.png?width=455&height=393&name=sehgraph_small-3.png)  
  


## Writing the script

The steps needed to write the script:

  1. For each thread in the process: 
     1. Retrieve the linear address of FS:[0] 
     2. Walk the exception registration record list and save the handler 
  2. Build the graph: 
     1. Allocate one node for each unique exception handler address 
     2. Add edges between the last exception handler and the current exception handler (so to create the chain visually) 
  3. Display the graph 



## Walking the exception registration records

In Win32, a new SEH is installed by filling an EXCEPTION_REGISTRATION_RECORD entry and linking it to the SEH chain (at FS:[0]).
    
    
    typedef struct _EXCEPTION_REGISTRATION_RECORD
    {
      struct _EXCEPTION_REGISTRATION_RECORD *Prev;
      PEXCEPTION_HANDLER       Handler;
    } EXCEPTION_REGISTRATION_RECORD;
    

Before walking the exception registration records, we need to get the base address of the FS selector.  
Fortunately, each debugger module provides a special callback in its _debugger_t_ structure:
    
    
    // Get information about the base of a segment register
    //   tid        - thread id
    //   sreg_value - value of the segment register
    //   answer     - pointer to the answer. can't be NULL.
    // 1-ok, 0-failed, -1-network error
    int (idaapi *thread_get_sreg_base)(
      thid_t tid,
      int sreg_value,
      ea_t *answer);
    

To use this callback, we will pass the FS selector value:
    
    
    def GetFsBase(tid):
        idc.SelectThread(tid)
        return idaapi.dbg_get_thread_sreg_base(tid, cpu.fs)
    

or in C:
    
    
    ea_t fs_base;
    dbg->thread_get_sreg_base(tid, fs_sel_value, &fs_base);
    

Now that we have the base, we can compute the linear address of the exception registration record list head by adding the base (fs_base) to the offset (which happens to be zero), thus: _fs_base + 0_  
With this knowledge, we can write a small loop to walk this list and extract the handlers:
    
    
    def GetExceptionChain(tid):
        fs_base = GetFsBase(tid)
        exc_rr = Dword(fs_base)
        result = []
        while exc_rr != 0xffffffff:
            prev    = Dword(exc_rr)
            handler = Dword(exc_rr + 4)
            exc_rr  = prev
            result.append(handler)
        return result
    

We do that for each thread:
    
    
        # Iterate through all function instructions and take only call instructions
        result = {}
        for tid in idautils.Threads():
            result[tid] = GetExceptionChain(tid)
    

## Building the graph

Building the graph is even simpler and can be done by subclassing the GraphViewer class and implementing the OnRefresh() and OnGetText() events.  
Here’s the simplified version of the graph building loop:
    
    
    def OnRefresh(self):
      self.Clear() # clear previous nodes
      addr_id = {}
      for (tid, chain) in self.result.items():
        # Add the thread node
        id_parent = self.AddNode("Thread %X" % tid)
        # Add each handler
        for handler in chain:
          # Get the node id given the handler address
          # We use an addr -> id dictionary
          # so that similar addresses get similar node id
          if not addr_id.has_key(handler):
            id = self.AddNode( hex(handler) )
            addr_id[handler] = id # add this ID
          else:
            id = addr_id[handler]
          # Link handlers to each other
          self.AddEdge(id_parent, id)
          # Now the parent node is this handler
          id_parent = id
      return True
    

## Putting it all together

The script will display the thread nodes and the handlers in different colors. Double clicking on a _handler_ node will jump to it in an IDA-View and double clicking on a _thread_ node will display the exception handlers in the message window. Here are some SEH graphs:

_IDA/Graphical version (idag.exe):_  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sehgraph_idag-thumb-3.png?width=490&height=393&name=sehgraph_idag-thumb-3.png)](</ida_pro/pix/sehgraph_idag1.html>)

_Visual Studio 2008 (devenv.exe):_  
[![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/sehgraph_devenv-thumb-3.png?width=490&height=393&name=sehgraph_devenv-thumb-3.png)](</ida_pro/pix/sehgraph_devenv.html>)

Please download the script from [here](</ida_pro/files/SEHGraph.py>) (you need IDAPython [r242](<http://code.google.com/p/idapython/updates/list>) and above).  
All comments and suggestions are welcome. You are also encouraged to share screenshots of interesting SEH graphs you run into.

---

## 5. 

**Date:** Posted: Sep 22, 2009  
**Link:** [https://hex-rays.com/blog/finding-instructions](https://hex-rays.com/blog/finding-instructions)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Finding instructions

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/finding-instructions>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/finding-instructions>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/finding-instructions>)

Elias Bachaalany ✦ Posted: Sep 22, 2009

![Finding instructions](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-52-18-9716-AM.png)

Searching for instructions and opcodes is a basic necessity for security researchers, therefore to address this issue IDA Pro provides many search facilities, among them we list:

  * Text search: Used to search the listing for text patterns (regular expressions are allowed). One can write a regular expression to find any assignment to the eax register (with the _mov_ instruction)  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/findinst_text-3.jpg)

  * Binary search: Allows you to search for binary patterns with wildcard support. It is also possible to search for strings alongside with the binary patterns.  
![](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/findinst_bin-3.jpg?width=429&height=361&name=findinst_bin-3.jpg)

  * Immediate search: Very useful to find constants and magic numbers used in the program. 
  * Please refer to the search menu for other search facilities 



None of the existing search facilities allow us to readily search for instructions and opcodes. In order to do that, one has to assemble the instruction in question then use the _Binary Search_ to find the pattern.

Each processor module in IDA can implement the _assemble_ notification callback:
    
    
    assemble,               // Assemble an instruction
                            // (display a warning if an error is found)
                            // args:
                            //  ea_t ea -  linear address of instruction
                            //  ea_t cs -  cs of instruction
                            //  ea_t ip -  ip of instruction
                            //  bool use32 - is 32bit segment?
                            //  const char *line - line to assemble
                            //  uchar *bin - pointer to output opcode buffer
                            // returns size of the instruction in bytes
    

Once this callback is implemented by the processor module one can then assemble instructions by calling the _ph.notify()_ with the _assemble_ notification code (please check this forum discussion [here](<//hex-rays.com/forum/viewtopic.php?f=8&t=2103&p=8834&hilit=assemble#p8834>)).  
Currently, only the _pc_ processor module implements this callback and provides a very basic assembler.  
We wrote a script that allows you to search for opcodes and assembly statements, so for example to find the “33 c0” (xor eax, eax), followed by “pop ebp” and followed by “ret” we could search like this:
    
    
    find("33 c0;pop ebp;ret")

That’s the script operation in brief:

  1. Do some input initial validation 
  2. Split the patterns 
  3. Loop: 
     1. Determine if the pattern is an assembly instruction or opcode list (using a simple regular expression) 
     2. If pattern is an instruction then assemble it 
     3. Accumulate the assembled (or converted opcodes) into a single buffer 
  4. Now that we have one single binary buffer we can search for it with FindBinary() 
  5. Display the result 



![](https://hex-rays.com/hubfs/Imported_Blog_Media/findinst_demo-3.jpg)  
  
The [script](</ida_pro/files/FindInstructions.py>) uses the Assemble() function (available in IdaPython [r233](<http://code.google.com/p/idapython>) and above). Comments and suggestions are welcome.

---

## 6. 

**Date:** Posted: Sep 4, 2009  
**Link:** [https://hex-rays.com/blog/driver-dispatch-table-viewer](https://hex-rays.com/blog/driver-dispatch-table-viewer)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Driver dispatch-table viewer

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/driver-dispatch-table-viewer>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/driver-dispatch-table-viewer>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/driver-dispatch-table-viewer>)

Elias Bachaalany ✦ Posted: Sep 4, 2009

![Driver dispatch-table viewer](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

With IDA, one can use the command line interface (CLI) not only to type scripting related commands but also to send debugger specific commands to the current debugger plugin.  
Although the topic mentions device drivers, you do not have to know much about drivers to learn something new from this post.  
  
For the sake of demonstration, we will start a [kernel debugging session](<http://www.hexblog.com/2009/01/kernel_debugging_with_ida.html>) with IDA/Windbg plugin and execute the !drvobj command:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/wdbg_cmd-3.jpg)  
We now have the dispatch table for the NTFS driver, but what if we want to display all the dispatch tables of all drivers and be able to easily browse the list with IDA?  
Before answering this, first let us see which debugger modules can receive commands through IDA’s CLI:

  * GDB: SendGDBMonitor() sends commands to GDB monitor 
  * Bochs: BochsCommand() sends [commands](<http://bochs.sourceforge.net/doc/docbook/user/internal-debugger.html>) to Bochs internal debugger (“info idt” and parse the result for instance?) 
  * WinDbg: WinDbgCommand() sends commands to the windbg debugger engine 



Please note that these commands are available only during the debugging session.  
Now that we know how to send commands to WinDbg, let us see how to answer the previous question:

  1. Get a list of loaded drivers: We can use IDA SDK (get_first_module()/get_next_module()) and/or scripting (GetFirstModule()/GetModuleName()). We can also use the “lm” command 
  2. Issue the “!drvobj DRVNAME” command and parse the result: In IDC we can simply write “auto s; s = WinDbgCommand(“!drvobj DRVNAME”)”. In Python we can use the Eval() to call an IDC function. 
  3. Parse and store the result: We can use regular expressions 
  4. Finally repeat the step 2 and 3 for all drivers. 



The end result is a simple IDAPython script that automates this task:  
![](https://hex-rays.com/hubfs/Imported_Blog_Media/wdbg_cmd_filter-3.jpg)  
Download the script from [here](</ida_pro/files/DrvsDispatch.py>). All comments and suggestions are welcome.

---

## 7. 

**Date:** Posted: Aug 7, 2009  
**Link:** [https://hex-rays.com/blog/javascript-for-ida-pro](https://hex-rays.com/blog/javascript-for-ida-pro)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Javascript for IDA Pro

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/javascript-for-ida-pro>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/javascript-for-ida-pro>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/javascript-for-ida-pro>)

Ilfak Guilfanov ✦ Posted: Aug 7, 2009

![Javascript for IDA Pro](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-59-54-9899-AM.png)

![](https://hex-rays.com/hubfs/Imported_Blog_Media/jshello-3.gif)Just a quick post to share the joy of having more expressiveness and freedom in IDA Pro. A few days ago we implemented a JavaScript plugin. This means that there is yet one more languauge to write scripts in IDA, and a very powerful one.

All usual methods of accessing the language work: you may execute scripts, standalone statements, or even completely replace IDC with JavaScript.

All IDC functions are availalble in JavaScript (in fact, we just exported them one-to-one). In the future, we will export IDA objects into JavaScript and this will make programming it even easier.

Download the plugin here:  
[/ida_pro/files/js.zip](</ida_pro/files/js.zip>)

If you notice anything unusual, send us a note, thank you!

Elias will blog more about the plugin in the coming days, and maybe present something handy, as he already [did](</blog/function-call-graph-plugin-sam>) in the past 😉

P.S. I subscribed to twitter a few days ago – it is so dynamic. Will probably switch to it, at least partially

---

## 8. 

**Date:** Posted: Jul 29, 2009  
**Link:** [https://hex-rays.com/blog/casts-are-bad](https://hex-rays.com/blog/casts-are-bad)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Casts are bad

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/casts-are-bad>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/casts-are-bad>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/casts-are-bad>)

Ilfak Guilfanov ✦ Posted: Jul 29, 2009

![Casts are bad](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-09-12-09-7789-AM.png)

Halvar and Dennis Elser recently [blogged ](<http://addxorrol.blogspot.com/2009/07/poking-around-msvidctldll.html>) about a serious vulnerability in the ATL libraries. A few days ago, Microsoft released an emergency “out-of-band” patch. Yes, the bug was that nasty, and since it is in a library, many MS Windows components were affected. Everyone who used the library should review their code and recompile with the corrected version.  
  
Microsoft posted more details about the vulnerability. As it turns out, it was quite difficult to catch the bug. Automatic tools, like source code scanners and fuzzers, could not detect it. The very bug consists of a small typo, and it is understandable that it could be missed during code reviews. Humans tend to skip punctuation marks and focus on “important” stuff. However, everything is important in the source code, even spacing.

Source code analysis tools and the compiler could not catch the bug because of a cast in the source code.  
Casts are a way to tell the compiler that an object of one kind should be treated as something different. For example, we have a 4 byte object and we can tell the compiler to treat it as a 2 byte object. The opposite is also possible: a 2 byte object can be treated as a 4 byte object. Naturally, when we use a cast, all responsibility is on us. A cast turns off all compiler checks and leaves us face-to-face with the cruel real world of bits and bytes. If we tell the compiler that the object is bigger than it is, all kinds of odd things can happen, including security breaches.

Since casts are that powerful, they should be used very sparingly. My attitude is to use them only there is no other choice. I know that my position differs from the mainstream. Many textbooks teach to use casts to “document” the code. Like, if a function expects a LONG parameter, they advise cast an uint variable to LONG. This is wrong. Let us check the vulnerable code, it is literally ridden with such casts:
    
    
    __int64 cbSize;
    hr = pStream->Read((void*) &cbSize, sizeof(cbSize), NULL);
    BYTE *pbArray;
    HRESULT hr = SafeArrayAccessData(psa, reinterpret_cast<LPVOID *>(&pbArray));
    hr = pStream->Read((void*)&pbArray, (ULONG)cbSize, NULL);
    

This is a really bad code snippet. It illustrates how casts are overused nowadays, which is a bad coding practice. 100% of statements above have casts; while the ratio of statements with casts to statements without casts should be 1:100 or less. Here’s the same code, with all casts removed (and the erroneous & removed):
    
    
    __int64 cbSize;
    hr = pStream->Read(&cbSize, sizeof(cbSize), NULL);
    BYTE *pbArray;
    HRESULT hr = SafeArrayAccessData(psa, &pbArray);
    hr = pStream->Read(pbArray, cbSize, NULL);
    

I did not try to compile it but it should pass all compiler checks and behave the same way. For me, the version without casts is shorter, simpler, easier to read. Why use any casts at all?

It is true that removing some casts leads to compiler warnings. Assignments that may truncate values, or comparisons of signed/unsigned integers are marked by many compilers as problematic. I would personally just turn off these warnings instead of damaging the source code with casts.

Casts to void* have no _raison d’etre_ at all. I always wonder why anyone would cast an address expression to void*.

It even gets funny. Some programmers go as far as marking function calls with void:
    
    
    (void)func();
    

This is their way of telling the reader that the return value of the function is not used. Well, isn’t the following simpler?
    
    
    func();
    

C/C++ are not verbose languages after all.

Well, to keep it short, I think the current practices of using casts should be revised. I would opt for using casts only when it is absolutely necessary. By the way, our [decompiler ](<http://www.hex-rays.com/products/decompiler/>)generates only necessary casts, with the exception of floating point-integer casts, which are always generated. In some cases even necessary casts are undesired (they obscure the program logic), so there is a command to temporarily hide all casts.

Keeping the source code simpler and shorter will allow us to avoid bugs like this: https://www.microsoft.com/technet/security/bulletin/ms09-034.mspx. Less casts also mean that source code analysis tools will perform better.

Less casts – better code.

---

## 9. 

**Date:** Posted: Jun 19, 2009  
**Link:** [https://hex-rays.com/blog/function-call-graph-plugin-sample](https://hex-rays.com/blog/function-call-graph-plugin-sample)

[Back](<https://hex-rays.com/blog>)

[Igor's tip of the week](<https://hex-rays.com/blog/tag/igors-tip-of-the-week>)

# Function call graph plugin sample

Copy link [ ](<https://twitter.com/intent/tweet?text=https://hex-rays.com/blog/function-call-graph-plugin-sample>) [ ](<https://mastodon.social/share?text=%20https://hex-rays.com/blog/function-call-graph-plugin-sample>) [ ](<https://www.linkedin.com/shareArticle?mini=true&url=https://hex-rays.com/blog/function-call-graph-plugin-sample>)

Elias Bachaalany ✦ Posted: Jun 19, 2009

![Function call graph plugin sample](https://hex-rays.com/hubfs/Imported_Blog_Media/bg-banner-Jun-18-2024-08-44-11-9294-AM.png)

IDA Pro already has a function call graph facility, nonetheless it employs WinGraph32.

Although it is helpful, it is not very useful just because it lacks interactivity.

![Wingraph function call graph](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fcg_wg32-3.jpg?width=400&height=300&name=fcg_wg32-3.jpg)

For demonstration purposes, we created a sample plugin that uses the graph functions from the SDK to achieve the same thing but with a bit more of interactivity: double-click to jump to a node, search node by name (also search next), etc…  
One can easily modify the plugin to also add a navigation stack or perhaps more filters.

![plugin function call graph](https://hex-rays.com/hs-fs/hubfs/Imported_Blog_Media/fcg_cg-3.jpg?width=400&height=300&name=fcg_cg-3.jpg)

After the plugin is installed it will create a new menu item in Views / Subviews / Function call graph, also make sure you right-click in the graph to see the options and hotkeys.

To recompile the plugin you need IDA Pro SDK version 5.5

Hope you find it useful!

[Click here to download the plugin](</ida_pro/pix/fcg_v1.zip>)

---

