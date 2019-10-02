---
src       : "src/plfa/part1/Quantifiers.lagda.md"
title     : "Quantifiers: Universals and existentials"
layout    : page
prev      : /Negation/
permalink : /Quantifiers/
next      : /Decidable/
---

{% raw %}<pre class="Agda"><a id="159" class="Keyword">module</a> <a id="166" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}" class="Module">plfa.part1.Quantifiers</a> <a id="189" class="Keyword">where</a>
</pre>{% endraw %}
This chapter introduces universal and existential quantification.

## Imports

{% raw %}<pre class="Agda"><a id="283" class="Keyword">import</a> <a id="290" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="328" class="Symbol">as</a> <a id="331" class="Module">Eq</a>
<a id="334" class="Keyword">open</a> <a id="339" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Binary.PropositionalEquality.html" class="Module">Eq</a> <a id="342" class="Keyword">using</a> <a id="348" class="Symbol">(</a><a id="349" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">_≡_</a><a id="352" class="Symbol">;</a> <a id="354" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a><a id="358" class="Symbol">)</a>
<a id="360" class="Keyword">open</a> <a id="365" class="Keyword">import</a> <a id="372" href="https://agda.github.io/agda-stdlib/v1.1/Data.Nat.html" class="Module">Data.Nat</a> <a id="381" class="Keyword">using</a> <a id="387" class="Symbol">(</a><a id="388" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="389" class="Symbol">;</a> <a id="391" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a><a id="395" class="Symbol">;</a> <a id="397" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a><a id="400" class="Symbol">;</a> <a id="402" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">_+_</a><a id="405" class="Symbol">;</a> <a id="407" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">_*_</a><a id="410" class="Symbol">)</a>
<a id="412" class="Keyword">open</a> <a id="417" class="Keyword">import</a> <a id="424" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html" class="Module">Relation.Nullary</a> <a id="441" class="Keyword">using</a> <a id="447" class="Symbol">(</a><a id="448" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬_</a><a id="450" class="Symbol">)</a>
<a id="452" class="Keyword">open</a> <a id="457" class="Keyword">import</a> <a id="464" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html" class="Module">Data.Product</a> <a id="477" class="Keyword">using</a> <a id="483" class="Symbol">(</a><a id="484" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">_×_</a><a id="487" class="Symbol">;</a> <a id="489" href="Agda.Builtin.Sigma.html#225" class="Field">proj₁</a><a id="494" class="Symbol">)</a> <a id="496" class="Keyword">renaming</a> <a id="505" class="Symbol">(</a><a id="506" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">_,_</a> <a id="510" class="Symbol">to</a> <a id="513" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">⟨_,_⟩</a><a id="518" class="Symbol">)</a>
<a id="520" class="Keyword">open</a> <a id="525" class="Keyword">import</a> <a id="532" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.html" class="Module">Data.Sum</a> <a id="541" class="Keyword">using</a> <a id="547" class="Symbol">(</a><a id="548" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">_⊎_</a><a id="551" class="Symbol">)</a>
<a id="553" class="Keyword">open</a> <a id="558" class="Keyword">import</a> <a id="565" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}" class="Module">plfa.part1.Isomorphism</a> <a id="588" class="Keyword">using</a> <a id="594" class="Symbol">(</a><a id="595" href="plfa.part1.Isomorphism.html#4365" class="Record Operator">_≃_</a><a id="598" class="Symbol">;</a> <a id="600" href="plfa.part1.Isomorphism.html#2684" class="Postulate">extensionality</a><a id="614" class="Symbol">)</a>
</pre>{% endraw %}

## Universals

We formalise universal quantification using the
dependent function type, which has appeared throughout this book.

Given a variable `x` of type `A` and a proposition `B x` which
contains `x` as a free variable, the universally quantified
proposition `∀ (x : A) → B x` holds if for every term `M` of type
`A` the proposition `B M` holds.  Here `B M` stands for
the proposition `B x` with each free occurrence of `x` replaced by
`M`.  Variable `x` appears free in `B x` but bound in
`∀ (x : A) → B x`.

Evidence that `∀ (x : A) → B x` holds is of the form

    λ (x : A) → N x

where `N x` is a term of type `B x`, and `N x` and `B x` both contain
a free variable `x` of type `A`.  Given a term `L` providing evidence
that `∀ (x : A) → B x` holds, and a term `M` of type `A`, the term `L
M` provides evidence that `B M` holds.  In other words, evidence that
`∀ (x : A) → B x` holds is a function that converts a term `M` of type
`A` into evidence that `B M` holds.

Put another way, if we know that `∀ (x : A) → B x` holds and that `M`
is a term of type `A` then we may conclude that `B M` holds:
{% raw %}<pre class="Agda"><a id="∀-elim"></a><a id="1736" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#1736" class="Function">∀-elim</a> <a id="1743" class="Symbol">:</a> <a id="1745" class="Symbol">∀</a> <a id="1747" class="Symbol">{</a><a id="1748" href="plfa.part1.Quantifiers.html#1748" class="Bound">A</a> <a id="1750" class="Symbol">:</a> <a id="1752" class="PrimitiveType">Set</a><a id="1755" class="Symbol">}</a> <a id="1757" class="Symbol">{</a><a id="1758" href="plfa.part1.Quantifiers.html#1758" class="Bound">B</a> <a id="1760" class="Symbol">:</a> <a id="1762" href="plfa.part1.Quantifiers.html#1748" class="Bound">A</a> <a id="1764" class="Symbol">→</a> <a id="1766" class="PrimitiveType">Set</a><a id="1769" class="Symbol">}</a>
  <a id="1773" class="Symbol">→</a> <a id="1775" class="Symbol">(</a><a id="1776" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#1776" class="Bound">L</a> <a id="1778" class="Symbol">:</a> <a id="1780" class="Symbol">∀</a> <a id="1782" class="Symbol">(</a><a id="1783" href="plfa.part1.Quantifiers.html#1783" class="Bound">x</a> <a id="1785" class="Symbol">:</a> <a id="1787" href="plfa.part1.Quantifiers.html#1748" class="Bound">A</a><a id="1788" class="Symbol">)</a> <a id="1790" class="Symbol">→</a> <a id="1792" href="plfa.part1.Quantifiers.html#1758" class="Bound">B</a> <a id="1794" href="plfa.part1.Quantifiers.html#1783" class="Bound">x</a><a id="1795" class="Symbol">)</a>
  <a id="1799" class="Symbol">→</a> <a id="1801" class="Symbol">(</a><a id="1802" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#1802" class="Bound">M</a> <a id="1804" class="Symbol">:</a> <a id="1806" href="plfa.part1.Quantifiers.html#1748" class="Bound">A</a><a id="1807" class="Symbol">)</a>
    <a id="1813" class="Comment">-----------------</a>
  <a id="1833" class="Symbol">→</a> <a id="1835" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#1758" class="Bound">B</a> <a id="1837" href="plfa.part1.Quantifiers.html#1802" class="Bound">M</a>
<a id="1839" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#1736" class="Function">∀-elim</a> <a id="1846" href="plfa.part1.Quantifiers.html#1846" class="Bound">L</a> <a id="1848" href="plfa.part1.Quantifiers.html#1848" class="Bound">M</a> <a id="1850" class="Symbol">=</a> <a id="1852" href="plfa.part1.Quantifiers.html#1846" class="Bound">L</a> <a id="1854" href="plfa.part1.Quantifiers.html#1848" class="Bound">M</a>
</pre>{% endraw %}As with `→-elim`, the rule corresponds to function application.

Functions arise as a special case of dependent functions,
where the range does not depend on a variable drawn from the domain.
When a function is viewed as evidence of implication, both its
argument and result are viewed as evidence, whereas when a dependent
function is viewed as evidence of a universal, its argument is viewed
as an element of a data type and its result is viewed as evidence of
a proposition that depends on the argument. This difference is largely
a matter of interpretation, since in Agda a value of a type and
evidence of a proposition are indistinguishable.

Dependent function types are sometimes referred to as dependent
products, because if `A` is a finite type with values `x₁ , ⋯ , xₙ`,
and if each of the types `B x₁ , ⋯ , B xₙ` has `m₁ , ⋯ , mₙ` distinct
members, then `∀ (x : A) → B x` has `m₁ * ⋯ * mₙ` members.  Indeed,
sometimes the notation `∀ (x : A) → B x` is replaced by a notation
such as `Π[ x ∈ A ] (B x)`, where `Π` stands for product.  However, we
will stick with the name dependent function, because (as we will see)
dependent product is ambiguous.


#### Exercise `∀-distrib-×` (recommended)

Show that universals distribute over conjunction:
{% raw %}<pre class="Agda"><a id="3118" class="Keyword">postulate</a>
  <a id="∀-distrib-×"></a><a id="3130" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3130" class="Postulate">∀-distrib-×</a> <a id="3142" class="Symbol">:</a> <a id="3144" class="Symbol">∀</a> <a id="3146" class="Symbol">{</a><a id="3147" href="plfa.part1.Quantifiers.html#3147" class="Bound">A</a> <a id="3149" class="Symbol">:</a> <a id="3151" class="PrimitiveType">Set</a><a id="3154" class="Symbol">}</a> <a id="3156" class="Symbol">{</a><a id="3157" href="plfa.part1.Quantifiers.html#3157" class="Bound">B</a> <a id="3159" href="plfa.part1.Quantifiers.html#3159" class="Bound">C</a> <a id="3161" class="Symbol">:</a> <a id="3163" href="plfa.part1.Quantifiers.html#3147" class="Bound">A</a> <a id="3165" class="Symbol">→</a> <a id="3167" class="PrimitiveType">Set</a><a id="3170" class="Symbol">}</a> <a id="3172" class="Symbol">→</a>
    <a id="3178" class="Symbol">(∀</a> <a id="3181" class="Symbol">(</a><a id="3182" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3182" class="Bound">x</a> <a id="3184" class="Symbol">:</a> <a id="3186" href="plfa.part1.Quantifiers.html#3147" class="Bound">A</a><a id="3187" class="Symbol">)</a> <a id="3189" class="Symbol">→</a> <a id="3191" href="plfa.part1.Quantifiers.html#3157" class="Bound">B</a> <a id="3193" href="plfa.part1.Quantifiers.html#3182" class="Bound">x</a> <a id="3195" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="3197" href="plfa.part1.Quantifiers.html#3159" class="Bound">C</a> <a id="3199" href="plfa.part1.Quantifiers.html#3182" class="Bound">x</a><a id="3200" class="Symbol">)</a> <a id="3202" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4365" class="Record Operator">≃</a> <a id="3204" class="Symbol">(∀</a> <a id="3207" class="Symbol">(</a><a id="3208" href="plfa.part1.Quantifiers.html#3208" class="Bound">x</a> <a id="3210" class="Symbol">:</a> <a id="3212" href="plfa.part1.Quantifiers.html#3147" class="Bound">A</a><a id="3213" class="Symbol">)</a> <a id="3215" class="Symbol">→</a> <a id="3217" href="plfa.part1.Quantifiers.html#3157" class="Bound">B</a> <a id="3219" href="plfa.part1.Quantifiers.html#3208" class="Bound">x</a><a id="3220" class="Symbol">)</a> <a id="3222" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="3224" class="Symbol">(∀</a> <a id="3227" class="Symbol">(</a><a id="3228" href="plfa.part1.Quantifiers.html#3228" class="Bound">x</a> <a id="3230" class="Symbol">:</a> <a id="3232" href="plfa.part1.Quantifiers.html#3147" class="Bound">A</a><a id="3233" class="Symbol">)</a> <a id="3235" class="Symbol">→</a> <a id="3237" href="plfa.part1.Quantifiers.html#3159" class="Bound">C</a> <a id="3239" href="plfa.part1.Quantifiers.html#3228" class="Bound">x</a><a id="3240" class="Symbol">)</a>
</pre>{% endraw %}Compare this with the result (`→-distrib-×`) in
Chapter [Connectives]({{ site.baseurl }}/Connectives/).

#### Exercise `⊎∀-implies-∀⊎` (practice)

Show that a disjunction of universals implies a universal of disjunctions:
{% raw %}<pre class="Agda"><a id="3472" class="Keyword">postulate</a>
  <a id="⊎∀-implies-∀⊎"></a><a id="3484" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3484" class="Postulate">⊎∀-implies-∀⊎</a> <a id="3498" class="Symbol">:</a> <a id="3500" class="Symbol">∀</a> <a id="3502" class="Symbol">{</a><a id="3503" href="plfa.part1.Quantifiers.html#3503" class="Bound">A</a> <a id="3505" class="Symbol">:</a> <a id="3507" class="PrimitiveType">Set</a><a id="3510" class="Symbol">}</a> <a id="3512" class="Symbol">{</a><a id="3513" href="plfa.part1.Quantifiers.html#3513" class="Bound">B</a> <a id="3515" href="plfa.part1.Quantifiers.html#3515" class="Bound">C</a> <a id="3517" class="Symbol">:</a> <a id="3519" href="plfa.part1.Quantifiers.html#3503" class="Bound">A</a> <a id="3521" class="Symbol">→</a> <a id="3523" class="PrimitiveType">Set</a><a id="3526" class="Symbol">}</a> <a id="3528" class="Symbol">→</a>
    <a id="3534" class="Symbol">(∀</a> <a id="3537" class="Symbol">(</a><a id="3538" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3538" class="Bound">x</a> <a id="3540" class="Symbol">:</a> <a id="3542" href="plfa.part1.Quantifiers.html#3503" class="Bound">A</a><a id="3543" class="Symbol">)</a> <a id="3545" class="Symbol">→</a> <a id="3547" href="plfa.part1.Quantifiers.html#3513" class="Bound">B</a> <a id="3549" href="plfa.part1.Quantifiers.html#3538" class="Bound">x</a><a id="3550" class="Symbol">)</a> <a id="3552" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="3554" class="Symbol">(∀</a> <a id="3557" class="Symbol">(</a><a id="3558" href="plfa.part1.Quantifiers.html#3558" class="Bound">x</a> <a id="3560" class="Symbol">:</a> <a id="3562" href="plfa.part1.Quantifiers.html#3503" class="Bound">A</a><a id="3563" class="Symbol">)</a> <a id="3565" class="Symbol">→</a> <a id="3567" href="plfa.part1.Quantifiers.html#3515" class="Bound">C</a> <a id="3569" href="plfa.part1.Quantifiers.html#3558" class="Bound">x</a><a id="3570" class="Symbol">)</a>  <a id="3573" class="Symbol">→</a>  <a id="3576" class="Symbol">∀</a> <a id="3578" class="Symbol">(</a><a id="3579" href="plfa.part1.Quantifiers.html#3579" class="Bound">x</a> <a id="3581" class="Symbol">:</a> <a id="3583" href="plfa.part1.Quantifiers.html#3503" class="Bound">A</a><a id="3584" class="Symbol">)</a> <a id="3586" class="Symbol">→</a> <a id="3588" href="plfa.part1.Quantifiers.html#3513" class="Bound">B</a> <a id="3590" href="plfa.part1.Quantifiers.html#3579" class="Bound">x</a> <a id="3592" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="3594" href="plfa.part1.Quantifiers.html#3515" class="Bound">C</a> <a id="3596" href="plfa.part1.Quantifiers.html#3579" class="Bound">x</a>
</pre>{% endraw %}Does the converse hold? If so, prove; if not, explain why.


#### Exercise `∀-×` (practice)

Consider the following type.
{% raw %}<pre class="Agda"><a id="3728" class="Keyword">data</a> <a id="Tri"></a><a id="3733" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3733" class="Datatype">Tri</a> <a id="3737" class="Symbol">:</a> <a id="3739" class="PrimitiveType">Set</a> <a id="3743" class="Keyword">where</a>
  <a id="Tri.aa"></a><a id="3751" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3751" class="InductiveConstructor">aa</a> <a id="3754" class="Symbol">:</a> <a id="3756" href="plfa.part1.Quantifiers.html#3733" class="Datatype">Tri</a>
  <a id="Tri.bb"></a><a id="3762" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3762" class="InductiveConstructor">bb</a> <a id="3765" class="Symbol">:</a> <a id="3767" href="plfa.part1.Quantifiers.html#3733" class="Datatype">Tri</a>
  <a id="Tri.cc"></a><a id="3773" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#3773" class="InductiveConstructor">cc</a> <a id="3776" class="Symbol">:</a> <a id="3778" href="plfa.part1.Quantifiers.html#3733" class="Datatype">Tri</a>
</pre>{% endraw %}Let `B` be a type indexed by `Tri`, that is `B : Tri → Set`.
Show that `∀ (x : Tri) → B x` is isomorphic to `B aa × B bb × B cc`.


## Existentials

Given a variable `x` of type `A` and a proposition `B x` which
contains `x` as a free variable, the existentially quantified
proposition `Σ[ x ∈ A ] B x` holds if for some term `M` of type
`A` the proposition `B M` holds.  Here `B M` stands for
the proposition `B x` with each free occurrence of `x` replaced by
`M`.  Variable `x` appears free in `B x` but bound in
`Σ[ x ∈ A ] B x`.

We formalise existential quantification by declaring a suitable
inductive type:
{% raw %}<pre class="Agda"><a id="4404" class="Keyword">data</a> <a id="Σ"></a><a id="4409" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#4409" class="Datatype">Σ</a> <a id="4411" class="Symbol">(</a><a id="4412" href="plfa.part1.Quantifiers.html#4412" class="Bound">A</a> <a id="4414" class="Symbol">:</a> <a id="4416" class="PrimitiveType">Set</a><a id="4419" class="Symbol">)</a> <a id="4421" class="Symbol">(</a><a id="4422" href="plfa.part1.Quantifiers.html#4422" class="Bound">B</a> <a id="4424" class="Symbol">:</a> <a id="4426" href="plfa.part1.Quantifiers.html#4412" class="Bound">A</a> <a id="4428" class="Symbol">→</a> <a id="4430" class="PrimitiveType">Set</a><a id="4433" class="Symbol">)</a> <a id="4435" class="Symbol">:</a> <a id="4437" class="PrimitiveType">Set</a> <a id="4441" class="Keyword">where</a>
  <a id="Σ.⟨_,_⟩"></a><a id="4449" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#4449" class="InductiveConstructor Operator">⟨_,_⟩</a> <a id="4455" class="Symbol">:</a> <a id="4457" class="Symbol">(</a><a id="4458" href="plfa.part1.Quantifiers.html#4458" class="Bound">x</a> <a id="4460" class="Symbol">:</a> <a id="4462" href="plfa.part1.Quantifiers.html#4412" class="Bound">A</a><a id="4463" class="Symbol">)</a> <a id="4465" class="Symbol">→</a> <a id="4467" href="plfa.part1.Quantifiers.html#4422" class="Bound">B</a> <a id="4469" href="plfa.part1.Quantifiers.html#4458" class="Bound">x</a> <a id="4471" class="Symbol">→</a> <a id="4473" href="plfa.part1.Quantifiers.html#4409" class="Datatype">Σ</a> <a id="4475" href="plfa.part1.Quantifiers.html#4412" class="Bound">A</a> <a id="4477" href="plfa.part1.Quantifiers.html#4422" class="Bound">B</a>
</pre>{% endraw %}We define a convenient syntax for existentials as follows:
{% raw %}<pre class="Agda"><a id="Σ-syntax"></a><a id="4546" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#4546" class="Function">Σ-syntax</a> <a id="4555" class="Symbol">=</a> <a id="4557" href="plfa.part1.Quantifiers.html#4409" class="Datatype">Σ</a>
<a id="4559" class="Keyword">infix</a> <a id="4565" class="Number">2</a> <a id="4567" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#4546" class="Function">Σ-syntax</a>
<a id="4576" class="Keyword">syntax</a> <a id="4583" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#4546" class="Function">Σ-syntax</a> <a id="4592" class="Bound">A</a> <a id="4594" class="Symbol">(λ</a> <a id="4597" class="Bound">x</a> <a id="4599" class="Symbol">→</a> <a id="4601" class="Bound">B</a><a id="4602" class="Symbol">)</a> <a id="4604" class="Symbol">=</a> <a id="4606" class="Function">Σ[</a> <a id="4609" class="Bound">x</a> <a id="4611" class="Function">∈</a> <a id="4613" class="Bound">A</a> <a id="4615" class="Function">]</a> <a id="4617" class="Bound">B</a>
</pre>{% endraw %}This is our first use of a syntax declaration, which specifies that
the term on the left may be written with the syntax on the right.
The special syntax is available only when the identifier
`Σ-syntax` is imported.

Evidence that `Σ[ x ∈ A ] B x` holds is of the form
`⟨ M , N ⟩` where `M` is a term of type `A`, and `N` is evidence
that `B M` holds.

Equivalently, we could also declare existentials as a record type:
{% raw %}<pre class="Agda"><a id="5046" class="Keyword">record</a> <a id="Σ′"></a><a id="5053" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#5053" class="Record">Σ′</a> <a id="5056" class="Symbol">(</a><a id="5057" href="plfa.part1.Quantifiers.html#5057" class="Bound">A</a> <a id="5059" class="Symbol">:</a> <a id="5061" class="PrimitiveType">Set</a><a id="5064" class="Symbol">)</a> <a id="5066" class="Symbol">(</a><a id="5067" href="plfa.part1.Quantifiers.html#5067" class="Bound">B</a> <a id="5069" class="Symbol">:</a> <a id="5071" href="plfa.part1.Quantifiers.html#5057" class="Bound">A</a> <a id="5073" class="Symbol">→</a> <a id="5075" class="PrimitiveType">Set</a><a id="5078" class="Symbol">)</a> <a id="5080" class="Symbol">:</a> <a id="5082" class="PrimitiveType">Set</a> <a id="5086" class="Keyword">where</a>
  <a id="5094" class="Keyword">field</a>
    <a id="Σ′.proj₁′"></a><a id="5104" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#5104" class="Field">proj₁′</a> <a id="5111" class="Symbol">:</a> <a id="5113" href="plfa.part1.Quantifiers.html#5057" class="Bound">A</a>
    <a id="Σ′.proj₂′"></a><a id="5119" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#5119" class="Field">proj₂′</a> <a id="5126" class="Symbol">:</a> <a id="5128" href="plfa.part1.Quantifiers.html#5067" class="Bound">B</a> <a id="5130" href="plfa.part1.Quantifiers.html#5104" class="Field">proj₁′</a>
</pre>{% endraw %}Here record construction

    record
      { proj₁′ = M
      ; proj₂′ = N
      }

corresponds to the term

    ⟨ M , N ⟩

where `M` is a term of type `A` and `N` is a term of type `B M`.

Products arise as a special case of existentials, where the second
component does not depend on a variable drawn from the first
component.  When a product is viewed as evidence of a conjunction,
both of its components are viewed as evidence, whereas when it is
viewed as evidence of an existential, the first component is viewed as
an element of a datatype and the second component is viewed as
evidence of a proposition that depends on the first component.  This
difference is largely a matter of interpretation, since in Agda a value
of a type and evidence of a proposition are indistinguishable.

Existentials are sometimes referred to as dependent sums,
because if `A` is a finite type with values `x₁ , ⋯ , xₙ`, and if
each of the types `B x₁ , ⋯ B xₙ` has `m₁ , ⋯ , mₙ` distinct members,
then `Σ[ x ∈ A ] B x` has `m₁ + ⋯ + mₙ` members, which explains the
choice of notation for existentials, since `Σ` stands for sum.

Existentials are sometimes referred to as dependent products, since
products arise as a special case.  However, that choice of names is
doubly confusing, since universals also have a claim to the name dependent
product and since existentials also have a claim to the name dependent sum.

A common notation for existentials is `∃` (analogous to `∀` for universals).
We follow the convention of the Agda standard library, and reserve this
notation for the case where the domain of the bound variable is left implicit:
{% raw %}<pre class="Agda"><a id="∃"></a><a id="6777" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6777" class="Function">∃</a> <a id="6779" class="Symbol">:</a> <a id="6781" class="Symbol">∀</a> <a id="6783" class="Symbol">{</a><a id="6784" href="plfa.part1.Quantifiers.html#6784" class="Bound">A</a> <a id="6786" class="Symbol">:</a> <a id="6788" class="PrimitiveType">Set</a><a id="6791" class="Symbol">}</a> <a id="6793" class="Symbol">(</a><a id="6794" href="plfa.part1.Quantifiers.html#6794" class="Bound">B</a> <a id="6796" class="Symbol">:</a> <a id="6798" href="plfa.part1.Quantifiers.html#6784" class="Bound">A</a> <a id="6800" class="Symbol">→</a> <a id="6802" class="PrimitiveType">Set</a><a id="6805" class="Symbol">)</a> <a id="6807" class="Symbol">→</a> <a id="6809" class="PrimitiveType">Set</a>
<a id="6813" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6777" class="Function">∃</a> <a id="6815" class="Symbol">{</a><a id="6816" href="plfa.part1.Quantifiers.html#6816" class="Bound">A</a><a id="6817" class="Symbol">}</a> <a id="6819" href="plfa.part1.Quantifiers.html#6819" class="Bound">B</a> <a id="6821" class="Symbol">=</a> <a id="6823" href="plfa.part1.Quantifiers.html#4409" class="Datatype">Σ</a> <a id="6825" href="plfa.part1.Quantifiers.html#6816" class="Bound">A</a> <a id="6827" href="plfa.part1.Quantifiers.html#6819" class="Bound">B</a>

<a id="∃-syntax"></a><a id="6830" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6830" class="Function">∃-syntax</a> <a id="6839" class="Symbol">=</a> <a id="6841" href="plfa.part1.Quantifiers.html#6777" class="Function">∃</a>
<a id="6843" class="Keyword">syntax</a> <a id="6850" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6830" class="Function">∃-syntax</a> <a id="6859" class="Symbol">(λ</a> <a id="6862" class="Bound">x</a> <a id="6864" class="Symbol">→</a> <a id="6866" class="Bound">B</a><a id="6867" class="Symbol">)</a> <a id="6869" class="Symbol">=</a> <a id="6871" class="Function">∃[</a> <a id="6874" class="Bound">x</a> <a id="6876" class="Function">]</a> <a id="6878" class="Bound">B</a>
</pre>{% endraw %}The special syntax is available only when the identifier `∃-syntax` is imported.
We will tend to use this syntax, since it is shorter and more familiar.

Given evidence that `∀ x → B x → C` holds, where `C` does not contain
`x` as a free variable, and given evidence that `∃[ x ] B x` holds, we
may conclude that `C` holds:
{% raw %}<pre class="Agda"><a id="∃-elim"></a><a id="7212" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7212" class="Function">∃-elim</a> <a id="7219" class="Symbol">:</a> <a id="7221" class="Symbol">∀</a> <a id="7223" class="Symbol">{</a><a id="7224" href="plfa.part1.Quantifiers.html#7224" class="Bound">A</a> <a id="7226" class="Symbol">:</a> <a id="7228" class="PrimitiveType">Set</a><a id="7231" class="Symbol">}</a> <a id="7233" class="Symbol">{</a><a id="7234" href="plfa.part1.Quantifiers.html#7234" class="Bound">B</a> <a id="7236" class="Symbol">:</a> <a id="7238" href="plfa.part1.Quantifiers.html#7224" class="Bound">A</a> <a id="7240" class="Symbol">→</a> <a id="7242" class="PrimitiveType">Set</a><a id="7245" class="Symbol">}</a> <a id="7247" class="Symbol">{</a><a id="7248" href="plfa.part1.Quantifiers.html#7248" class="Bound">C</a> <a id="7250" class="Symbol">:</a> <a id="7252" class="PrimitiveType">Set</a><a id="7255" class="Symbol">}</a>
  <a id="7259" class="Symbol">→</a> <a id="7261" class="Symbol">(∀</a> <a id="7264" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7264" class="Bound">x</a> <a id="7266" class="Symbol">→</a> <a id="7268" href="plfa.part1.Quantifiers.html#7234" class="Bound">B</a> <a id="7270" href="plfa.part1.Quantifiers.html#7264" class="Bound">x</a> <a id="7272" class="Symbol">→</a> <a id="7274" href="plfa.part1.Quantifiers.html#7248" class="Bound">C</a><a id="7275" class="Symbol">)</a>
  <a id="7279" class="Symbol">→</a> <a id="7281" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6830" class="Function">∃[</a> <a id="7284" href="plfa.part1.Quantifiers.html#7284" class="Bound">x</a> <a id="7286" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="7288" href="plfa.part1.Quantifiers.html#7234" class="Bound">B</a> <a id="7290" href="plfa.part1.Quantifiers.html#7284" class="Bound">x</a>
    <a id="7296" class="Comment">---------------</a>
  <a id="7314" class="Symbol">→</a> <a id="7316" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7248" class="Bound">C</a>
<a id="7318" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7212" class="Function">∃-elim</a> <a id="7325" href="plfa.part1.Quantifiers.html#7325" class="Bound">f</a> <a id="7327" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="7329" href="plfa.part1.Quantifiers.html#7329" class="Bound">x</a> <a id="7331" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="7333" href="plfa.part1.Quantifiers.html#7333" class="Bound">y</a> <a id="7335" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a> <a id="7337" class="Symbol">=</a> <a id="7339" href="plfa.part1.Quantifiers.html#7325" class="Bound">f</a> <a id="7341" href="plfa.part1.Quantifiers.html#7329" class="Bound">x</a> <a id="7343" href="plfa.part1.Quantifiers.html#7333" class="Bound">y</a>
</pre>{% endraw %}In other words, if we know for every `x` of type `A` that `B x`
implies `C`, and we know for some `x` of type `A` that `B x` holds,
then we may conclude that `C` holds.  This is because we may
instantiate that proof that `∀ x → B x → C` to any value `x` of type
`A` and any `y` of type `B x`, and exactly such values are provided by
the evidence for `∃[ x ] B x`.

Indeed, the converse also holds, and the two together form an isomorphism:
{% raw %}<pre class="Agda"><a id="∀∃-currying"></a><a id="7793" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7793" class="Function">∀∃-currying</a> <a id="7805" class="Symbol">:</a> <a id="7807" class="Symbol">∀</a> <a id="7809" class="Symbol">{</a><a id="7810" href="plfa.part1.Quantifiers.html#7810" class="Bound">A</a> <a id="7812" class="Symbol">:</a> <a id="7814" class="PrimitiveType">Set</a><a id="7817" class="Symbol">}</a> <a id="7819" class="Symbol">{</a><a id="7820" href="plfa.part1.Quantifiers.html#7820" class="Bound">B</a> <a id="7822" class="Symbol">:</a> <a id="7824" href="plfa.part1.Quantifiers.html#7810" class="Bound">A</a> <a id="7826" class="Symbol">→</a> <a id="7828" class="PrimitiveType">Set</a><a id="7831" class="Symbol">}</a> <a id="7833" class="Symbol">{</a><a id="7834" href="plfa.part1.Quantifiers.html#7834" class="Bound">C</a> <a id="7836" class="Symbol">:</a> <a id="7838" class="PrimitiveType">Set</a><a id="7841" class="Symbol">}</a>
  <a id="7845" class="Symbol">→</a> <a id="7847" class="Symbol">(∀</a> <a id="7850" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7850" class="Bound">x</a> <a id="7852" class="Symbol">→</a> <a id="7854" href="plfa.part1.Quantifiers.html#7820" class="Bound">B</a> <a id="7856" href="plfa.part1.Quantifiers.html#7850" class="Bound">x</a> <a id="7858" class="Symbol">→</a> <a id="7860" href="plfa.part1.Quantifiers.html#7834" class="Bound">C</a><a id="7861" class="Symbol">)</a> <a id="7863" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4365" class="Record Operator">≃</a> <a id="7865" class="Symbol">(</a><a id="7866" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="7869" href="plfa.part1.Quantifiers.html#7869" class="Bound">x</a> <a id="7871" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="7873" href="plfa.part1.Quantifiers.html#7820" class="Bound">B</a> <a id="7875" href="plfa.part1.Quantifiers.html#7869" class="Bound">x</a> <a id="7877" class="Symbol">→</a> <a id="7879" href="plfa.part1.Quantifiers.html#7834" class="Bound">C</a><a id="7880" class="Symbol">)</a>
<a id="7882" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7793" class="Function">∀∃-currying</a> <a id="7894" class="Symbol">=</a>
  <a id="7898" class="Keyword">record</a>
    <a id="7909" class="Symbol">{</a> <a id="7911" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4405" class="Field">to</a>      <a id="7919" class="Symbol">=</a>  <a id="7922" class="Symbol">λ{</a> <a id="7925" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7925" class="Bound">f</a> <a id="7927" class="Symbol">→</a> <a id="7929" class="Symbol">λ{</a> <a id="7932" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="7934" href="plfa.part1.Quantifiers.html#7934" class="Bound">x</a> <a id="7936" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="7938" href="plfa.part1.Quantifiers.html#7938" class="Bound">y</a> <a id="7940" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a> <a id="7942" class="Symbol">→</a> <a id="7944" href="plfa.part1.Quantifiers.html#7925" class="Bound">f</a> <a id="7946" href="plfa.part1.Quantifiers.html#7934" class="Bound">x</a> <a id="7948" href="plfa.part1.Quantifiers.html#7938" class="Bound">y</a> <a id="7950" class="Symbol">}}</a>
    <a id="7957" class="Symbol">;</a> <a id="7959" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4422" class="Field">from</a>    <a id="7967" class="Symbol">=</a>  <a id="7970" class="Symbol">λ{</a> <a id="7973" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#7973" class="Bound">g</a> <a id="7975" class="Symbol">→</a> <a id="7977" class="Symbol">λ{</a> <a id="7980" href="plfa.part1.Quantifiers.html#7980" class="Bound">x</a> <a id="7982" class="Symbol">→</a> <a id="7984" class="Symbol">λ{</a> <a id="7987" href="plfa.part1.Quantifiers.html#7987" class="Bound">y</a> <a id="7989" class="Symbol">→</a> <a id="7991" href="plfa.part1.Quantifiers.html#7973" class="Bound">g</a> <a id="7993" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="7995" href="plfa.part1.Quantifiers.html#7980" class="Bound">x</a> <a id="7997" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="7999" href="plfa.part1.Quantifiers.html#7987" class="Bound">y</a> <a id="8001" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a> <a id="8003" class="Symbol">}}}</a>
    <a id="8011" class="Symbol">;</a> <a id="8013" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4439" class="Field">from∘to</a> <a id="8021" class="Symbol">=</a>  <a id="8024" class="Symbol">λ{</a> <a id="8027" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#8027" class="Bound">f</a> <a id="8029" class="Symbol">→</a> <a id="8031" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="8036" class="Symbol">}</a>
    <a id="8042" class="Symbol">;</a> <a id="8044" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4481" class="Field">to∘from</a> <a id="8052" class="Symbol">=</a>  <a id="8055" class="Symbol">λ{</a> <a id="8058" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#8058" class="Bound">g</a> <a id="8060" class="Symbol">→</a> <a id="8062" href="plfa.part1.Isomorphism.html#2684" class="Postulate">extensionality</a> <a id="8077" class="Symbol">λ{</a> <a id="8080" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="8082" href="plfa.part1.Quantifiers.html#8082" class="Bound">x</a> <a id="8084" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="8086" href="plfa.part1.Quantifiers.html#8086" class="Bound">y</a> <a id="8088" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a> <a id="8090" class="Symbol">→</a> <a id="8092" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="8097" class="Symbol">}}</a>
    <a id="8104" class="Symbol">}</a>
</pre>{% endraw %}The result can be viewed as a generalisation of currying.  Indeed, the code to
establish the isomorphism is identical to what we wrote when discussing
[implication]({{ site.baseurl }}/Connectives/#implication).

#### Exercise `∃-distrib-⊎` (recommended)

Show that existentials distribute over disjunction:
{% raw %}<pre class="Agda"><a id="8421" class="Keyword">postulate</a>
  <a id="∃-distrib-⊎"></a><a id="8433" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#8433" class="Postulate">∃-distrib-⊎</a> <a id="8445" class="Symbol">:</a> <a id="8447" class="Symbol">∀</a> <a id="8449" class="Symbol">{</a><a id="8450" href="plfa.part1.Quantifiers.html#8450" class="Bound">A</a> <a id="8452" class="Symbol">:</a> <a id="8454" class="PrimitiveType">Set</a><a id="8457" class="Symbol">}</a> <a id="8459" class="Symbol">{</a><a id="8460" href="plfa.part1.Quantifiers.html#8460" class="Bound">B</a> <a id="8462" href="plfa.part1.Quantifiers.html#8462" class="Bound">C</a> <a id="8464" class="Symbol">:</a> <a id="8466" href="plfa.part1.Quantifiers.html#8450" class="Bound">A</a> <a id="8468" class="Symbol">→</a> <a id="8470" class="PrimitiveType">Set</a><a id="8473" class="Symbol">}</a> <a id="8475" class="Symbol">→</a>
    <a id="8481" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6830" class="Function">∃[</a> <a id="8484" href="plfa.part1.Quantifiers.html#8484" class="Bound">x</a> <a id="8486" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="8488" class="Symbol">(</a><a id="8489" href="plfa.part1.Quantifiers.html#8460" class="Bound">B</a> <a id="8491" href="plfa.part1.Quantifiers.html#8484" class="Bound">x</a> <a id="8493" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="8495" href="plfa.part1.Quantifiers.html#8462" class="Bound">C</a> <a id="8497" href="plfa.part1.Quantifiers.html#8484" class="Bound">x</a><a id="8498" class="Symbol">)</a> <a id="8500" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4365" class="Record Operator">≃</a> <a id="8502" class="Symbol">(</a><a id="8503" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="8506" href="plfa.part1.Quantifiers.html#8506" class="Bound">x</a> <a id="8508" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="8510" href="plfa.part1.Quantifiers.html#8460" class="Bound">B</a> <a id="8512" href="plfa.part1.Quantifiers.html#8506" class="Bound">x</a><a id="8513" class="Symbol">)</a> <a id="8515" href="https://agda.github.io/agda-stdlib/v1.1/Data.Sum.Base.html#612" class="Datatype Operator">⊎</a> <a id="8517" class="Symbol">(</a><a id="8518" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="8521" href="plfa.part1.Quantifiers.html#8521" class="Bound">x</a> <a id="8523" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="8525" href="plfa.part1.Quantifiers.html#8462" class="Bound">C</a> <a id="8527" href="plfa.part1.Quantifiers.html#8521" class="Bound">x</a><a id="8528" class="Symbol">)</a>
</pre>{% endraw %}
#### Exercise `∃×-implies-×∃` (practice)

Show that an existential of conjunctions implies a conjunction of existentials:
{% raw %}<pre class="Agda"><a id="8661" class="Keyword">postulate</a>
  <a id="∃×-implies-×∃"></a><a id="8673" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#8673" class="Postulate">∃×-implies-×∃</a> <a id="8687" class="Symbol">:</a> <a id="8689" class="Symbol">∀</a> <a id="8691" class="Symbol">{</a><a id="8692" href="plfa.part1.Quantifiers.html#8692" class="Bound">A</a> <a id="8694" class="Symbol">:</a> <a id="8696" class="PrimitiveType">Set</a><a id="8699" class="Symbol">}</a> <a id="8701" class="Symbol">{</a><a id="8702" href="plfa.part1.Quantifiers.html#8702" class="Bound">B</a> <a id="8704" href="plfa.part1.Quantifiers.html#8704" class="Bound">C</a> <a id="8706" class="Symbol">:</a> <a id="8708" href="plfa.part1.Quantifiers.html#8692" class="Bound">A</a> <a id="8710" class="Symbol">→</a> <a id="8712" class="PrimitiveType">Set</a><a id="8715" class="Symbol">}</a> <a id="8717" class="Symbol">→</a>
    <a id="8723" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6830" class="Function">∃[</a> <a id="8726" href="plfa.part1.Quantifiers.html#8726" class="Bound">x</a> <a id="8728" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="8730" class="Symbol">(</a><a id="8731" href="plfa.part1.Quantifiers.html#8702" class="Bound">B</a> <a id="8733" href="plfa.part1.Quantifiers.html#8726" class="Bound">x</a> <a id="8735" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="8737" href="plfa.part1.Quantifiers.html#8704" class="Bound">C</a> <a id="8739" href="plfa.part1.Quantifiers.html#8726" class="Bound">x</a><a id="8740" class="Symbol">)</a> <a id="8742" class="Symbol">→</a> <a id="8744" class="Symbol">(</a><a id="8745" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="8748" href="plfa.part1.Quantifiers.html#8748" class="Bound">x</a> <a id="8750" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="8752" href="plfa.part1.Quantifiers.html#8702" class="Bound">B</a> <a id="8754" href="plfa.part1.Quantifiers.html#8748" class="Bound">x</a><a id="8755" class="Symbol">)</a> <a id="8757" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1162" class="Function Operator">×</a> <a id="8759" class="Symbol">(</a><a id="8760" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="8763" href="plfa.part1.Quantifiers.html#8763" class="Bound">x</a> <a id="8765" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="8767" href="plfa.part1.Quantifiers.html#8704" class="Bound">C</a> <a id="8769" href="plfa.part1.Quantifiers.html#8763" class="Bound">x</a><a id="8770" class="Symbol">)</a>
</pre>{% endraw %}Does the converse hold? If so, prove; if not, explain why.

#### Exercise `∃-⊎` (practice)

Let `Tri` and `B` be as in Exercise `∀-×`.
Show that `∃[ x ] B x` is isomorphic to `B aa ⊎ B bb ⊎ B cc`.


## An existential example

Recall the definitions of `even` and `odd` from
Chapter [Relations]({{ site.baseurl }}/Relations/):
{% raw %}<pre class="Agda"><a id="9106" class="Keyword">data</a> <a id="even"></a><a id="9111" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9111" class="Datatype">even</a> <a id="9116" class="Symbol">:</a> <a id="9118" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a> <a id="9120" class="Symbol">→</a> <a id="9122" class="PrimitiveType">Set</a>
<a id="9126" class="Keyword">data</a> <a id="odd"></a><a id="9131" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9131" class="Datatype">odd</a>  <a id="9136" class="Symbol">:</a> <a id="9138" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a> <a id="9140" class="Symbol">→</a> <a id="9142" class="PrimitiveType">Set</a>

<a id="9147" class="Keyword">data</a> <a id="9152" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9111" class="Datatype">even</a> <a id="9157" class="Keyword">where</a>

  <a id="even.even-zero"></a><a id="9166" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9166" class="InductiveConstructor">even-zero</a> <a id="9176" class="Symbol">:</a> <a id="9178" href="plfa.part1.Quantifiers.html#9111" class="Datatype">even</a> <a id="9183" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a>

  <a id="even.even-suc"></a><a id="9191" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9191" class="InductiveConstructor">even-suc</a> <a id="9200" class="Symbol">:</a> <a id="9202" class="Symbol">∀</a> <a id="9204" class="Symbol">{</a><a id="9205" href="plfa.part1.Quantifiers.html#9205" class="Bound">n</a> <a id="9207" class="Symbol">:</a> <a id="9209" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="9210" class="Symbol">}</a>
    <a id="9216" class="Symbol">→</a> <a id="9218" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9131" class="Datatype">odd</a> <a id="9222" href="plfa.part1.Quantifiers.html#9205" class="Bound">n</a>
      <a id="9230" class="Comment">------------</a>
    <a id="9247" class="Symbol">→</a> <a id="9249" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9111" class="Datatype">even</a> <a id="9254" class="Symbol">(</a><a id="9255" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9259" href="plfa.part1.Quantifiers.html#9205" class="Bound">n</a><a id="9260" class="Symbol">)</a>

<a id="9263" class="Keyword">data</a> <a id="9268" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9131" class="Datatype">odd</a> <a id="9272" class="Keyword">where</a>
  <a id="odd.odd-suc"></a><a id="9280" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9280" class="InductiveConstructor">odd-suc</a> <a id="9288" class="Symbol">:</a> <a id="9290" class="Symbol">∀</a> <a id="9292" class="Symbol">{</a><a id="9293" href="plfa.part1.Quantifiers.html#9293" class="Bound">n</a> <a id="9295" class="Symbol">:</a> <a id="9297" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="9298" class="Symbol">}</a>
    <a id="9304" class="Symbol">→</a> <a id="9306" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9111" class="Datatype">even</a> <a id="9311" href="plfa.part1.Quantifiers.html#9293" class="Bound">n</a>
      <a id="9319" class="Comment">-----------</a>
    <a id="9335" class="Symbol">→</a> <a id="9337" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9131" class="Datatype">odd</a> <a id="9341" class="Symbol">(</a><a id="9342" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="9346" href="plfa.part1.Quantifiers.html#9293" class="Bound">n</a><a id="9347" class="Symbol">)</a>
</pre>{% endraw %}A number is even if it is zero or the successor of an odd number, and
odd if it is the successor of an even number.

We will show that a number is even if and only if it is twice some
other number, and odd if and only if it is one more than twice
some other number.  In other words, we will show:

`even n`   iff   `∃[ m ] (    m * 2 ≡ n)`

`odd  n`   iff   `∃[ m ] (1 + m * 2 ≡ n)`

By convention, one tends to write constant factors first and to put
the constant term in a sum last. Here we've reversed each of those
conventions, because doing so eases the proof.

Here is the proof in the forward direction:
{% raw %}<pre class="Agda"><a id="even-∃"></a><a id="9968" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9968" class="Function">even-∃</a> <a id="9975" class="Symbol">:</a> <a id="9977" class="Symbol">∀</a> <a id="9979" class="Symbol">{</a><a id="9980" href="plfa.part1.Quantifiers.html#9980" class="Bound">n</a> <a id="9982" class="Symbol">:</a> <a id="9984" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="9985" class="Symbol">}</a> <a id="9987" class="Symbol">→</a> <a id="9989" href="plfa.part1.Quantifiers.html#9111" class="Datatype">even</a> <a id="9994" href="plfa.part1.Quantifiers.html#9980" class="Bound">n</a> <a id="9996" class="Symbol">→</a> <a id="9998" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="10001" href="plfa.part1.Quantifiers.html#10001" class="Bound">m</a> <a id="10003" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="10005" class="Symbol">(</a>    <a id="10010" href="plfa.part1.Quantifiers.html#10001" class="Bound">m</a> <a id="10012" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="10014" class="Number">2</a> <a id="10016" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="10018" href="plfa.part1.Quantifiers.html#9980" class="Bound">n</a><a id="10019" class="Symbol">)</a>
<a id="odd-∃"></a><a id="10021" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#10021" class="Function">odd-∃</a>  <a id="10028" class="Symbol">:</a> <a id="10030" class="Symbol">∀</a> <a id="10032" class="Symbol">{</a><a id="10033" href="plfa.part1.Quantifiers.html#10033" class="Bound">n</a> <a id="10035" class="Symbol">:</a> <a id="10037" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="10038" class="Symbol">}</a> <a id="10040" class="Symbol">→</a>  <a id="10043" href="plfa.part1.Quantifiers.html#9131" class="Datatype">odd</a> <a id="10047" href="plfa.part1.Quantifiers.html#10033" class="Bound">n</a> <a id="10049" class="Symbol">→</a> <a id="10051" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="10054" href="plfa.part1.Quantifiers.html#10054" class="Bound">m</a> <a id="10056" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="10058" class="Symbol">(</a><a id="10059" class="Number">1</a> <a id="10061" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="10063" href="plfa.part1.Quantifiers.html#10054" class="Bound">m</a> <a id="10065" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="10067" class="Number">2</a> <a id="10069" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="10071" href="plfa.part1.Quantifiers.html#10033" class="Bound">n</a><a id="10072" class="Symbol">)</a>

<a id="10075" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9968" class="Function">even-∃</a> <a id="10082" href="plfa.part1.Quantifiers.html#9166" class="InductiveConstructor">even-zero</a>                       <a id="10114" class="Symbol">=</a>  <a id="10117" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="10119" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a> <a id="10124" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="10126" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="10131" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>
<a id="10133" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#9968" class="Function">even-∃</a> <a id="10140" class="Symbol">(</a><a id="10141" href="plfa.part1.Quantifiers.html#9191" class="InductiveConstructor">even-suc</a> <a id="10150" href="plfa.part1.Quantifiers.html#10150" class="Bound">o</a><a id="10151" class="Symbol">)</a> <a id="10153" class="Keyword">with</a> <a id="10158" href="plfa.part1.Quantifiers.html#10021" class="Function">odd-∃</a> <a id="10164" href="plfa.part1.Quantifiers.html#10150" class="Bound">o</a>
<a id="10166" class="Symbol">...</a>                    <a id="10189" class="Symbol">|</a> <a id="10191" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#4449" class="InductiveConstructor Operator">⟨</a> <a id="10193" href="plfa.part1.Quantifiers.html#10193" class="Bound">m</a> <a id="10195" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="10197" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="10202" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>  <a id="10205" class="Symbol">=</a>  <a id="10208" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="10210" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="10214" href="plfa.part1.Quantifiers.html#10193" class="Bound">m</a> <a id="10216" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="10218" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="10223" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>

<a id="10226" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#10021" class="Function">odd-∃</a>  <a id="10233" class="Symbol">(</a><a id="10234" href="plfa.part1.Quantifiers.html#9280" class="InductiveConstructor">odd-suc</a> <a id="10242" href="plfa.part1.Quantifiers.html#10242" class="Bound">e</a><a id="10243" class="Symbol">)</a>  <a id="10246" class="Keyword">with</a> <a id="10251" href="plfa.part1.Quantifiers.html#9968" class="Function">even-∃</a> <a id="10258" href="plfa.part1.Quantifiers.html#10242" class="Bound">e</a>
<a id="10260" class="Symbol">...</a>                    <a id="10283" class="Symbol">|</a> <a id="10285" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#4449" class="InductiveConstructor Operator">⟨</a> <a id="10287" href="plfa.part1.Quantifiers.html#10287" class="Bound">m</a> <a id="10289" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="10291" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="10296" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>  <a id="10299" class="Symbol">=</a>  <a id="10302" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="10304" href="plfa.part1.Quantifiers.html#10287" class="Bound">m</a> <a id="10306" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="10308" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="10313" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>
</pre>{% endraw %}We define two mutually recursive functions. Given
evidence that `n` is even or odd, we return a
number `m` and evidence that `m * 2 ≡ n` or `1 + m * 2 ≡ n`.
We induct over the evidence that `n` is even or odd:

* If the number is even because it is zero, then we return a pair
consisting of zero and the evidence that twice zero is zero.

* If the number is even because it is one more than an odd number,
then we apply the induction hypothesis to give a number `m` and
evidence that `1 + m * 2 ≡ n`. We return a pair consisting of `suc m`
and evidence that `suc m * 2 ≡ suc n`, which is immediate after
substituting for `n`.

* If the number is odd because it is the successor of an even number,
then we apply the induction hypothesis to give a number `m` and
evidence that `m * 2 ≡ n`. We return a pair consisting of `suc m` and
evidence that `1 + m * 2 ≡ suc n`, which is immediate after
substituting for `n`.

This completes the proof in the forward direction.

Here is the proof in the reverse direction:
{% raw %}<pre class="Agda"><a id="∃-even"></a><a id="11333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#11333" class="Function">∃-even</a> <a id="11340" class="Symbol">:</a> <a id="11342" class="Symbol">∀</a> <a id="11344" class="Symbol">{</a><a id="11345" href="plfa.part1.Quantifiers.html#11345" class="Bound">n</a> <a id="11347" class="Symbol">:</a> <a id="11349" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="11350" class="Symbol">}</a> <a id="11352" class="Symbol">→</a> <a id="11354" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="11357" href="plfa.part1.Quantifiers.html#11357" class="Bound">m</a> <a id="11359" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="11361" class="Symbol">(</a>    <a id="11366" href="plfa.part1.Quantifiers.html#11357" class="Bound">m</a> <a id="11368" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="11370" class="Number">2</a> <a id="11372" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="11374" href="plfa.part1.Quantifiers.html#11345" class="Bound">n</a><a id="11375" class="Symbol">)</a> <a id="11377" class="Symbol">→</a> <a id="11379" href="plfa.part1.Quantifiers.html#9111" class="Datatype">even</a> <a id="11384" href="plfa.part1.Quantifiers.html#11345" class="Bound">n</a>
<a id="∃-odd"></a><a id="11386" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#11386" class="Function">∃-odd</a>  <a id="11393" class="Symbol">:</a> <a id="11395" class="Symbol">∀</a> <a id="11397" class="Symbol">{</a><a id="11398" href="plfa.part1.Quantifiers.html#11398" class="Bound">n</a> <a id="11400" class="Symbol">:</a> <a id="11402" href="Agda.Builtin.Nat.html#165" class="Datatype">ℕ</a><a id="11403" class="Symbol">}</a> <a id="11405" class="Symbol">→</a> <a id="11407" href="plfa.part1.Quantifiers.html#6830" class="Function">∃[</a> <a id="11410" href="plfa.part1.Quantifiers.html#11410" class="Bound">m</a> <a id="11412" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="11414" class="Symbol">(</a><a id="11415" class="Number">1</a> <a id="11417" href="Agda.Builtin.Nat.html#298" class="Primitive Operator">+</a> <a id="11419" href="plfa.part1.Quantifiers.html#11410" class="Bound">m</a> <a id="11421" href="Agda.Builtin.Nat.html#501" class="Primitive Operator">*</a> <a id="11423" class="Number">2</a> <a id="11425" href="Agda.Builtin.Equality.html#125" class="Datatype Operator">≡</a> <a id="11427" href="plfa.part1.Quantifiers.html#11398" class="Bound">n</a><a id="11428" class="Symbol">)</a> <a id="11430" class="Symbol">→</a>  <a id="11433" href="plfa.part1.Quantifiers.html#9131" class="Datatype">odd</a> <a id="11437" href="plfa.part1.Quantifiers.html#11398" class="Bound">n</a>

<a id="11440" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#11333" class="Function">∃-even</a> <a id="11447" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a>  <a id="11450" href="Agda.Builtin.Nat.html#183" class="InductiveConstructor">zero</a> <a id="11455" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="11457" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="11462" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>  <a id="11465" class="Symbol">=</a>  <a id="11468" href="plfa.part1.Quantifiers.html#9166" class="InductiveConstructor">even-zero</a>
<a id="11478" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#11333" class="Function">∃-even</a> <a id="11485" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="11487" href="Agda.Builtin.Nat.html#196" class="InductiveConstructor">suc</a> <a id="11491" href="plfa.part1.Quantifiers.html#11491" class="Bound">m</a> <a id="11493" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="11495" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="11500" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>  <a id="11503" class="Symbol">=</a>  <a id="11506" href="plfa.part1.Quantifiers.html#9191" class="InductiveConstructor">even-suc</a> <a id="11515" class="Symbol">(</a><a id="11516" href="plfa.part1.Quantifiers.html#11386" class="Function">∃-odd</a> <a id="11522" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="11524" href="plfa.part1.Quantifiers.html#11491" class="Bound">m</a> <a id="11526" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="11528" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="11533" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a><a id="11534" class="Symbol">)</a>

<a id="11537" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#11386" class="Function">∃-odd</a>  <a id="11544" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a>     <a id="11550" href="plfa.part1.Quantifiers.html#11550" class="Bound">m</a> <a id="11552" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="11554" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="11559" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a>  <a id="11562" class="Symbol">=</a>  <a id="11565" href="plfa.part1.Quantifiers.html#9280" class="InductiveConstructor">odd-suc</a> <a id="11573" class="Symbol">(</a><a id="11574" href="plfa.part1.Quantifiers.html#11333" class="Function">∃-even</a> <a id="11581" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="11583" href="plfa.part1.Quantifiers.html#11550" class="Bound">m</a> <a id="11585" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="11587" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="11592" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a><a id="11593" class="Symbol">)</a>
</pre>{% endraw %}Given a number that is twice some other number we must show it is
even, and a number that is one more than twice some other number we
must show it is odd.  We induct over the evidence of the existential,
and in the even case consider the two possibilities for the number
that is doubled:

- In the even case for `zero`, we must show `zero * 2` is even, which
follows by `even-zero`.

- In the even case for `suc n`, we must show `suc m * 2` is even.  The
inductive hypothesis tells us that `1 + m * 2` is odd, from which the
desired result follows by `even-suc`.

- In the odd case, we must show `1 + m * 2` is odd.  The inductive
hypothesis tell us that `m * 2` is even, from which the desired result
follows by `odd-suc`.

This completes the proof in the backward direction.

#### Exercise `∃-even-odd` (practice)

How do the proofs become more difficult if we replace `m * 2` and `1 + m * 2`
by `2 * m` and `2 * m + 1`?  Rewrite the proofs of `∃-even` and `∃-odd` when
restated in this way.

{% raw %}<pre class="Agda"><a id="12598" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}
#### Exercise `∃-|-≤` (practice)

Show that `y ≤ z` holds if and only if there exists a `x` such that
`x + y ≡ z`.

{% raw %}<pre class="Agda"><a id="12746" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}

## Existentials, Universals, and Negation

Negation of an existential is isomorphic to the universal
of a negation.  Considering that existentials are generalised
disjunction and universals are generalised conjunction, this
result is analogous to the one which tells us that negation
of a disjunction is isomorphic to a conjunction of negations:
{% raw %}<pre class="Agda"><a id="¬∃≃∀¬"></a><a id="13125" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#13125" class="Function">¬∃≃∀¬</a> <a id="13131" class="Symbol">:</a> <a id="13133" class="Symbol">∀</a> <a id="13135" class="Symbol">{</a><a id="13136" href="plfa.part1.Quantifiers.html#13136" class="Bound">A</a> <a id="13138" class="Symbol">:</a> <a id="13140" class="PrimitiveType">Set</a><a id="13143" class="Symbol">}</a> <a id="13145" class="Symbol">{</a><a id="13146" href="plfa.part1.Quantifiers.html#13146" class="Bound">B</a> <a id="13148" class="Symbol">:</a> <a id="13150" href="plfa.part1.Quantifiers.html#13136" class="Bound">A</a> <a id="13152" class="Symbol">→</a> <a id="13154" class="PrimitiveType">Set</a><a id="13157" class="Symbol">}</a>
  <a id="13161" class="Symbol">→</a> <a id="13163" class="Symbol">(</a><a id="13164" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="13166" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6830" class="Function">∃[</a> <a id="13169" href="plfa.part1.Quantifiers.html#13169" class="Bound">x</a> <a id="13171" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="13173" href="plfa.part1.Quantifiers.html#13146" class="Bound">B</a> <a id="13175" href="plfa.part1.Quantifiers.html#13169" class="Bound">x</a><a id="13176" class="Symbol">)</a> <a id="13178" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4365" class="Record Operator">≃</a> <a id="13180" class="Symbol">∀</a> <a id="13182" href="plfa.part1.Quantifiers.html#13182" class="Bound">x</a> <a id="13184" class="Symbol">→</a> <a id="13186" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="13188" href="plfa.part1.Quantifiers.html#13146" class="Bound">B</a> <a id="13190" href="plfa.part1.Quantifiers.html#13182" class="Bound">x</a>
<a id="13192" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#13125" class="Function">¬∃≃∀¬</a> <a id="13198" class="Symbol">=</a>
  <a id="13202" class="Keyword">record</a>
    <a id="13213" class="Symbol">{</a> <a id="13215" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4405" class="Field">to</a>      <a id="13223" class="Symbol">=</a>  <a id="13226" class="Symbol">λ{</a> <a id="13229" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#13229" class="Bound">¬∃xy</a> <a id="13234" href="plfa.part1.Quantifiers.html#13234" class="Bound">x</a> <a id="13236" href="plfa.part1.Quantifiers.html#13236" class="Bound">y</a> <a id="13238" class="Symbol">→</a> <a id="13240" href="plfa.part1.Quantifiers.html#13229" class="Bound">¬∃xy</a> <a id="13245" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="13247" href="plfa.part1.Quantifiers.html#13234" class="Bound">x</a> <a id="13249" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="13251" href="plfa.part1.Quantifiers.html#13236" class="Bound">y</a> <a id="13253" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a> <a id="13255" class="Symbol">}</a>
    <a id="13261" class="Symbol">;</a> <a id="13263" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4422" class="Field">from</a>    <a id="13271" class="Symbol">=</a>  <a id="13274" class="Symbol">λ{</a> <a id="13277" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#13277" class="Bound">∀¬xy</a> <a id="13282" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="13284" href="plfa.part1.Quantifiers.html#13284" class="Bound">x</a> <a id="13286" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="13288" href="plfa.part1.Quantifiers.html#13288" class="Bound">y</a> <a id="13290" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a> <a id="13292" class="Symbol">→</a> <a id="13294" href="plfa.part1.Quantifiers.html#13277" class="Bound">∀¬xy</a> <a id="13299" href="plfa.part1.Quantifiers.html#13284" class="Bound">x</a> <a id="13301" href="plfa.part1.Quantifiers.html#13288" class="Bound">y</a> <a id="13303" class="Symbol">}</a>
    <a id="13309" class="Symbol">;</a> <a id="13311" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4439" class="Field">from∘to</a> <a id="13319" class="Symbol">=</a>  <a id="13322" class="Symbol">λ{</a> <a id="13325" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#13325" class="Bound">¬∃xy</a> <a id="13330" class="Symbol">→</a> <a id="13332" href="plfa.part1.Isomorphism.html#2684" class="Postulate">extensionality</a> <a id="13347" class="Symbol">λ{</a> <a id="13350" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟨</a> <a id="13352" href="plfa.part1.Quantifiers.html#13352" class="Bound">x</a> <a id="13354" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">,</a> <a id="13356" href="plfa.part1.Quantifiers.html#13356" class="Bound">y</a> <a id="13358" href="plfa.part1.Quantifiers.html#4449" class="InductiveConstructor Operator">⟩</a> <a id="13360" class="Symbol">→</a> <a id="13362" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="13367" class="Symbol">}</a> <a id="13369" class="Symbol">}</a>
    <a id="13375" class="Symbol">;</a> <a id="13377" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Isomorphism.md %}{% raw %}#4481" class="Field">to∘from</a> <a id="13385" class="Symbol">=</a>  <a id="13388" class="Symbol">λ{</a> <a id="13391" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#13391" class="Bound">∀¬xy</a> <a id="13396" class="Symbol">→</a> <a id="13398" href="Agda.Builtin.Equality.html#182" class="InductiveConstructor">refl</a> <a id="13403" class="Symbol">}</a>
    <a id="13409" class="Symbol">}</a>
</pre>{% endraw %}In the `to` direction, we are given a value `¬∃xy` of type
`¬ ∃[ x ] B x`, and need to show that given a value
`x` that `¬ B x` follows, in other words, from
a value `y` of type `B x` we can derive false.  Combining
`x` and `y` gives us a value `⟨ x , y ⟩` of type `∃[ x ] B x`,
and applying `¬∃xy` to that yields a contradiction.

In the `from` direction, we are given a value `∀¬xy` of type
`∀ x → ¬ B x`, and need to show that from a value `⟨ x , y ⟩`
of type `∃[ x ] B x` we can derive false.  Applying `∀¬xy`
to `x` gives a value of type `¬ B x`, and applying that to `y` yields
a contradiction.

The two inverse proofs are straightforward, where one direction
requires extensionality.


#### Exercise `∃¬-implies-¬∀` (recommended)

Show that existential of a negation implies negation of a universal:
{% raw %}<pre class="Agda"><a id="14226" class="Keyword">postulate</a>
  <a id="∃¬-implies-¬∀"></a><a id="14238" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#14238" class="Postulate">∃¬-implies-¬∀</a> <a id="14252" class="Symbol">:</a> <a id="14254" class="Symbol">∀</a> <a id="14256" class="Symbol">{</a><a id="14257" href="plfa.part1.Quantifiers.html#14257" class="Bound">A</a> <a id="14259" class="Symbol">:</a> <a id="14261" class="PrimitiveType">Set</a><a id="14264" class="Symbol">}</a> <a id="14266" class="Symbol">{</a><a id="14267" href="plfa.part1.Quantifiers.html#14267" class="Bound">B</a> <a id="14269" class="Symbol">:</a> <a id="14271" href="plfa.part1.Quantifiers.html#14257" class="Bound">A</a> <a id="14273" class="Symbol">→</a> <a id="14275" class="PrimitiveType">Set</a><a id="14278" class="Symbol">}</a>
    <a id="14284" class="Symbol">→</a> <a id="14286" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#6830" class="Function">∃[</a> <a id="14289" href="plfa.part1.Quantifiers.html#14289" class="Bound">x</a> <a id="14291" href="plfa.part1.Quantifiers.html#6830" class="Function">]</a> <a id="14293" class="Symbol">(</a><a id="14294" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="14296" href="plfa.part1.Quantifiers.html#14267" class="Bound">B</a> <a id="14298" href="plfa.part1.Quantifiers.html#14289" class="Bound">x</a><a id="14299" class="Symbol">)</a>
      <a id="14307" class="Comment">--------------</a>
    <a id="14326" class="Symbol">→</a> <a id="14328" href="https://agda.github.io/agda-stdlib/v1.1/Relation.Nullary.html#535" class="Function Operator">¬</a> <a id="14330" class="Symbol">(∀</a> <a id="14333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/part1/Quantifiers.md %}{% raw %}#14333" class="Bound">x</a> <a id="14335" class="Symbol">→</a> <a id="14337" href="plfa.part1.Quantifiers.html#14267" class="Bound">B</a> <a id="14339" href="plfa.part1.Quantifiers.html#14333" class="Bound">x</a><a id="14340" class="Symbol">)</a>
</pre>{% endraw %}Does the converse hold? If so, prove; if not, explain why.


#### Exercise `Bin-isomorphism` (stretch) {#Bin-isomorphism}

Recall that Exercises
[Bin]({{ site.baseurl }}/Naturals/#Bin),
[Bin-laws]({{ site.baseurl }}/Induction/#Bin-laws), and
[Bin-predicates]({{ site.baseurl }}/Relations/#Bin-predicates)
define a datatype `Bin` of bitstrings representing natural numbers,
and asks you to define the following functions and predicates:

    to   : ℕ → Bin
    from : Bin → ℕ
    Can  : Bin → Set

And to establish the following properties:

    from (to n) ≡ n

    ----------
    Can (to n)

    Can b
    ---------------
    to (from b) ≡ b

Using the above, establish that there is an isomorphism between `ℕ` and
`∃[ b ](Can b)`.

{% raw %}<pre class="Agda"><a id="15084" class="Comment">-- Your code goes here</a>
</pre>{% endraw %}

## Standard library

Definitions similar to those in this chapter can be found in the standard library:
{% raw %}<pre class="Agda"><a id="15221" class="Keyword">import</a> <a id="15228" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html" class="Module">Data.Product</a> <a id="15241" class="Keyword">using</a> <a id="15247" class="Symbol">(</a><a id="15248" href="Agda.Builtin.Sigma.html#139" class="Record">Σ</a><a id="15249" class="Symbol">;</a> <a id="15251" href="Agda.Builtin.Sigma.html#209" class="InductiveConstructor Operator">_,_</a><a id="15254" class="Symbol">;</a> <a id="15256" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1364" class="Function">∃</a><a id="15257" class="Symbol">;</a> <a id="15259" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#911" class="Function">Σ-syntax</a><a id="15267" class="Symbol">;</a> <a id="15269" href="https://agda.github.io/agda-stdlib/v1.1/Data.Product.html#1783" class="Function">∃-syntax</a><a id="15277" class="Symbol">)</a>
</pre>{% endraw %}

## Unicode

This chapter uses the following unicode:

    Π  U+03A0  GREEK CAPITAL LETTER PI (\Pi)
    Σ  U+03A3  GREEK CAPITAL LETTER SIGMA (\Sigma)
    ∃  U+2203  THERE EXISTS (\ex, \exists)