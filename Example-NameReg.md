Name Registration plugin example (v0.1)

```qml
import QtQuick 2.0
import QtQuick.Controls 1.0;
import QtQuick.Layouts 1.0;
import Ethereum 1.0

ApplicationWindow {
	minimumWidth: 500
	maximumWidth: 500
	maximumHeight: 300
	minimumHeight: 300

	title: "NameReg"
	TextField {
		id: amount
		placeholderText: "Amount"
		anchors.topMargin: 5
	}
	TextField {
		id: receiver
		placeholderText: "Recipient"
		anchors.top: textField.bottom
		anchors.topMargin: 5
	}
	Button {
		anchors.top: receiver.bottom
		anchors.topMargin: 5
		text: "Send"
		onClicked: {
			eth.createTx("11f62328e131dbb05ce4c73a3de3c7ab1c84a163", amount.text, receiver + "\n" + "")
		}
	}
}
```