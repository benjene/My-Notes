# Observer
interface Subject {
    method addObserver(Observer observer)
    method removeObserver(Observer observer)
    method notifyObservers()
}

interface Observer {
    method update(Subject subject, Object arg)
}

class ConcreteSubject implements Subject {
    private List<Observer> observers = new List<Observer>()
    private Object state

    method addObserver(Observer observer) {
        observers.add(observer)
    }

    method removeObserver(Observer observer) {
        observers.remove(observer)
    }

    method notifyObservers() {
        for (observer in observers) {
            observer.update(this, state)
        }
    }

    method setState(Object newState) {
        state = newState
        notifyObservers()
    }

    method getState() {
        return state
    }
}

class ConcreteObserver implements Observer {
    method update(Subject subject, Object arg) {
        if (subject instanceof ConcreteSubject) {
            // React to the update from subject, using state if necessary
            Object state = ((ConcreteSubject) subject).getState()
            // Handle the state
        }
    }
}
