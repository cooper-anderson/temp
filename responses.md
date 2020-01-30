# Lab 6 - Normalization

`FD1: Store → Address, Manager`
> **Question 1:** Why is FD1 a functional dependency for the Promotions relation?
```
Every store has a single Address and Manager, therefore the Store attribute uniquely determines those two.
```

> **Question 2:** Does this violate 3NF? Does it violate BCNF? Justify both answers.
```
This FD does not comply with BCNF because you cannot determine a unique Promotion with just the name of the store.
Because the FD is not BCNF and the RHS is not contained in the key, it does not comply with 3NF either.
```

---
`FD2: Manager → ManagerPh`
> **Question 3:** Why is FD2 a functional dependency for the Promotions relation?
```
Each Manager has a single phone number associating them with a specific store. Therefore, the Manager uniquely
determines the phone number.
```

> **Question 4:** Does this violate 3NF? Does it violate BCNF? Justify both answers.
```
This FD does not comply with BCNF because from Manager, you can only get to ManagerPh.
Because the FD is not BCNF and ManagerPh is not contained in the key, it does not comply with 3NF either.
```

---
`FD3: Promotion Name → Promotion Terms`
> **Question 5:** Why is FD3 a functional dependency for the Promotions relation?
```
Because each Promotion only has a single set of Promotion Terms, is can be uniquely determined, and is thus an FD.
```

> **Question 6:** Does this violate 3NF? Does it violate BCNF? Justify both answers.
```
This FD does not comply with BCNF because the same set of Terms can be used on multiple Stores.
Because the FD is not BCNF and Promotion Terms is not in the key, it does not comply with 3NF either.
```

---
`FD4: Store, Promotion Name → Total Sales For Promotion`
> **Question 7:** Why is FD4 a functional dependency for the Promotions relation?
```
For each Promotion at a given Store, there is a unique Total Sale. Therefore, it is an FD.
```

> **Question 8:** Does this violate 3NF? Does it violate BCNF? Justify both answers.
```
This FD complies with BCNF because using the two attributes on the left you can get to any attribute in the table.
Because this FD complies with BCNF, it also complies with 3NF.
```

---
> **Question 9:** Calculate the closure {Store}+ of FD1.
```
Store → {Store, Addr, Mgr, MgrPh}
MgrPh is included because it can be uniquely determined using the Mgr.
```

> **Question 10:** Continue the process of decomposing the relations that still violate third
normal form until all the relations are in third normal form. Give the final relations and
attributes in Relation(Attribute1, Attribute2, ...) format. Show your work following the
process from class. (Hint: Your final decomposition should have four relations, two of
which have two attributes and two of which have three attributes.)
```
'*' specifies that the given relation will be used in the final model.

Store → {Store, Addr, Mgr, MgrPh}
(Addr, Mgr, MgrPh  (Store)  PName, PTerms, PTotal)
  R1(Store, Addr, Mgr, MgrPh)
  R2(Store, PName, PTerms, PTotal)

Mgr → MgrPh
(MgrPh  (Mgr)  Store, Addr)
* R1(Mgr, MgrPh)
* R2(Store, Addr, Mgr)

PName → PTerms
(PTerms  (PName)  Store, PTotal)
* R1(PName, PTerms)
* R2(Store, PName, PTotal)

Store(Name, Addr, Mgr)
Manager(Name, Phone)
Promotion(Name, Terms)
HasPromo(Store, PName, PTotal)
```

---
> **Question 12:** Copy and paste the four SQL commands for populating your new tables into your text document.
```sql
INSERT INTO Manager(Name, Phone)
	SELECT DISTINCT Manager, ManagerPhone
		FROM Promotions

INSERT INTO Store(Name, Addr, Mgr)
	SELECT DISTINCT Store, Address, Manager
		FROM Promotions

INSERT INTO Promotion(Name, Terms)
	SELECT DISTINCT PromotionName, PromotionTerms
		FROM Promotions

INSERT INTO HasPromo(Store, PName, PTotal)
	SELECT DISTINCT Store, PromotionName, TotalSales
		FROM Promotions
```