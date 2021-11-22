Vorlesung5 Montag 22.November 2021
==================================

- ein Kosntruktor gibt immer das Objekt selbst zurück, keinen Typen
- man keinen Konstruktor in einem anderen Konstruktor der gleichen Klasse aufrufen

- Überladen des = Operators (Zuweisungsoperator):

.. code-block:: C++

	{
	public:
		crational& operator = (const crational &);
	};

	
	crational& crational::operator = (const crational & c)
	{
		this->numerator = c.numerator;
		this->denominator = c.denominator;
		this->shorten();
		return *this;
	}

- ist eine Wandlungsfähigkeit zwischen int und crational vorhanden? ja -> temporäre Variable wird überblendet und erst der Kosntruktor aufgerufen, anschließend wird der WErt zugewiesen

.. image:: _static/images/22November/22november.png
	:width: 800
	:alt:
