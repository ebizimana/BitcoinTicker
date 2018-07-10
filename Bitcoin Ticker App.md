# BitCoin App Step by Step
## Set up some variables
1. currencyArray[] that the titles of the array you want to use
1. currencySymbolArray [] that contains the symbol for all the currency in the same order as the currencies
1. currencySelected just an empty string
## How to set up and use the UIPicker Class
1. In the `ViewController.swift` code file. Inherte `UIPickerViewDataSource` and `UIPickerViewDelagate`
1. Set `delegate` and `dataSource` of the UIPickerView to `self` inside `viewDidLoad()` function
    ```swift
      currencyPicker.delegate = self
      currenyPicker.dataSource = self
    ```
### Let's take care of the `UIPickerViewDelagate` Pre-requisite requirements
1. Add `numbeerOfComponents(in)` function to determine how many columns we want in our picker.
    * Add `return 1` line after it loads, for one column
1. Add `numberOfRowsInComponet` function to add the number of rows
    * Add `return currencyArray.count`
    * `currencyArray.count` countain the number of all the currency we are going to use
1. Add `titleForRow` function to fill rows
    * Add `return currencyArray[rows]`
    * the `row` is an `Int` variable that is provided from the `titleForRow` function
    * the `row` increases from zero to the total number of row that UIPicker has
    * `currencyArray[rows]` outputs the name that is on that index depending what `row` is.
1. Add `didSelectRow` to do sonething what's the user has pressed on a row
    * Add a way to print the name of the currncy
      `print(currencyArray[row])`

## Construct The API URL
1. The API ditect to get the current value of a BitCoin in currency of your choising, you have to send the URL like this:
    * https://apiv2.bitcoinaverage.com/indices/global/ticker/**BTC\<Currency>**
    * Because we have different currency names we have to keep changing this url to the correct value for the selected    currency
1. To achieve this we have to break down the url into two parts
    ```swift
      func didSelectRow() {// swift function, it will autocomplete
          finalURL = baseURL + currencyArray[row]
        }
    ```
## Set up your Cocoapods
1. You need to set up Almorfire and SwiftyJSON libraries
1. Check the Swift_Packeges.md to see how to set them up

## Networking
1. import Alamofire
1. import SwiftyJSON
1. Add `getBitcoinData(url: String)` function
1. set up the Alomfire function as standart

    `Almorfire.request(url: url, .get).responseJSON{
    response in
    }`
    * Inside the Almofire do:
      1. Add `let bitcoinJSON: JSON = JSON(response.result.value!)`
      1. Add `self.updateBitcoinData(json: bitcoinJSON)`
## Parse JSON Data
**Note:** Remember you can always go to the JSON Editor Online to see how you data is broken down.
1. Create `updateBitcoinData` function
    ```swift
      func updateBitcoinData(json:JSON ){
        if let bitcoinResult = json[].double{
          bitcoinPriceLabel.text = String(bitcoinResult)
        }
      }
    ```

## Update the User Interface
* Just update the `bitcoinPriceLabel` in the JSON parsing function
* Call `getBitcoinData() url: finalURL` in `didSelectRow` function
1. At the end `didSelectRow` function will look like this
```swift
  func didSelectRow() {// swift function, it will autocomplete
      finalURL = baseURL + currencyArray[row]
      currencySelected = currencySymbolArray[row]
      getBitcoinData(url: finalURL)
    }
```
