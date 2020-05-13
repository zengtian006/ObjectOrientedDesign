```python

class ParkingLot:
    instance = None

    class __OnlyOne:
        def __init__(self, name, address):
            self.__name = name
            self.__address = address
            
    def __init__(self, name, address):
        if not ParkingLot.instance:
            ParkingLot.instance = ParkingLot.__OnlyOne(name, address)
        else:
            ParkingLot.instance.__name = name
            arkingLot.instance.__address = address
            
    def __getattr__(self, name):
        return getattr(self.instance, name)