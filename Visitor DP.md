Drawbacks:
- Breaks encapsulation

Delegate choosing proper method to object itself
- Each object must accept visitor, tell it what visiting method must be executed

Classes of visitor DP
Visitor interface
- Has classes visitTypeA, visitTypeB, etc.
Concrete visitor implements Visitor

Visitable interface
- Has accept(Visitor)

ConcreteElement

```
acceptVisitor(visitor)
	doThing();
	return visitor.visitTypeA(); // sometimes 
```


# Code
## 
```
interface Visitor {
	public double visit(Liquor liquorItem);
	public double visit(AnotherClass classItem);
}
interface Visitable {
	public double accept(Visitor visitor);
}

class taxVisitor implements Visitor { //a tax visitor eg
	public taxVisitor() {}

	public double visit(Liquor liquorItem) {
		//returns taxable price
		return 0.25*liquorItem.getPrice();
	}
	
	public double visit(Anotherclass classItem) {
			//returns taxable price
			return 0.18*classItem.getPrice();
		}
}

class ReduceTaxesVisitor implements visitor {
	public ReduceTaxesVisitor() {}

	public double visit(Liquor liquorItem) {
		//returns taxable price
		return 0.10*liquorItem.getPrice();
	}
	
	public double visit(Anotherclass classItem) {
			//returns taxable price
			return 0.4*classItem.getPrice();
		}
}

class Liquor implements Visitable {
	private double price;
	
	Liquor(double item){
		price = item;
	}

	@Override
	public double accept(Visitor visitor) {
		return visitor.visit(this);
	}
	
public double getPrice () {
	return price;
}

main() {
	TaxVisitor getTax = new TaxVisitor();
	Liquor vodka = new Liquor(20);

	print(vodka.accept(taxCalc));
	
}

```