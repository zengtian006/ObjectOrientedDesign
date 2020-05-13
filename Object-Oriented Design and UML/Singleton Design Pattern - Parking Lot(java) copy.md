```java

public class ParkingLot{
    public static ParkingLot parkingLot = null
    public ParkingLot(){

    }

    public static ParkingLot getInstance(){
        if (parkingLot == null){
            parkingLot = new ParkingLot()
        }
        return parkingLot
    }
}