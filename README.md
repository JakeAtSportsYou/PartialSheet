# PartialSheet

A custom SwiftUI modifier to present a Partial Modal Sheet based on his content size.

## iPhone Preview

<img src="https://user-images.githubusercontent.com/11211914/68700576-6c100580-0585-11ea-847b-99f0450311a4.gif" width="250"><img src="https://user-images.githubusercontent.com/11211914/68700574-6c100580-0585-11ea-9727-8a02ec36b118.gif" width="250">

## iPad Preview
<img src="https://user-images.githubusercontent.com/11211914/79673521-af019380-821d-11ea-82f5-49d75e83d7c0.png" width="500">

## Mac Preview
<img src="https://user-images.githubusercontent.com/11211914/79673482-7eb9f500-821d-11ea-93e0-60fc32e554ee.png" width="600">


## Features

#### Availables
- \[x]  Slidable and dismissable with drag gesture
- \[x]  Variable height based on his content
- \[x]  Customizable colors
- \[x] Keyboard compatibility
- \[x]  Landscape compatibility
- \[x]  iOS compatibility
- \[x] iPad compatibility
- \[x] Mac compatibility

#### Nice to have
- \[ ] ScrcrollView and List compatibility: as soon as Apple adds some API to handle better ScrollViews

## Installation

#### Requirements
- iOS 13.0+ / macOS 10.15+
- Xcode 11.2+
- Swift 5+

#### Via Swift Package Manager

In Xcode 11 or grater, in you project, select: `File > Swift Packages > Add Pacakage Dependency`.

In the search bar type **PartialSheet** and when you find the package, with the **next** button you can proceed with the installation.

If you can't find anything in the panel of the Swift Packages you probably haven't added yet your github account.
You can do that under the **Preferences** panel of your Xcode, in the **Accounts** section.

##  How to Use

To use the **Partial Sheet** just attach the new modifier:

```Swift
YourView
.partialSheet(
    presented: Binding<Bool>, 
    style: PartialSheetStyle = PartialSheetStyle.default()
    onDismiss: (() -> Void)? = nil, 
    view: @escaping () -> SheetContent) -> some View where SheetContent : View
```

If you want a starting point copy in your ContentView file the following code:

```Swift
import SwiftUI
import PartialSheet

struct ContentView: View {
    @State private var modalPresented: Bool = false

    var body: some View {
        NavigationView {
            VStack(alignment: .leading) {
                HStack {
                    Spacer()
                }
                Text("""
                Hi, this is the Partial Sheet modifier.

                On iPhone devices it allows you to dispaly a totally custom sheet with a relative height based on his content.
                In this way the sheet will cover the screen only for the space it will need.

                On iPad and Mac devices it will present a normal .sheet view.
                """)
                Spacer()
                HStack {
                    Spacer()
                    Button(action: {
                        self.modalPresented = true
                    }, label: {
                        Text("Display the Partial Shehet")
                    })
                        .padding()
                    Spacer()
                }
                Spacer()
                Spacer()
            }
            .padding()
            .navigationBarTitle("Partial Sheet")
        }
        .navigationViewStyle(StackNavigationViewStyle())
        .partialSheet(presented: $modalPresented, onDismiss: {
            print("dismissed")
        }) {
            SheetView()
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

struct SheetView: View {
    @State private var longer: Bool = false
    @State private var text: String = "some text"

    var body: some View {
        VStack {
            Group {
                Text("Settings Panel")
                    .font(.headline)

                TextField("TextField", text: self.$text)
                    .padding(8)
                    .overlay(
                        RoundedRectangle(cornerRadius: 4)
                        .stroke(Color(UIColor.systemGray2), lineWidth: 1)
                    )
                    
                Toggle(isOn: self.$longer) {
                    Text("Advanced")
                }
            }
            .padding()
            .frame(height: 50)
            if self.longer {
                VStack {
                    Text("More settings here...")
                }
                .frame(height: 200)
            }
        }
    }
}
```


