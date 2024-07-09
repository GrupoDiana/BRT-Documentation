# Examples

## Code Annotations Axamples

### Codeblocks

Some `code` goes here.

### Plain codeblock

A plain codeblock:

```
template <typename T>
bool ofApp::AddListenerModel(const std::string& _listenerID) {
	brtManager.BeginSetup();
	std::shared_ptr<T>listenerModel = brtManager.CreateListenerModel<T>(_listenerID);
	brtManager.EndSetup();
	if (listenerModel != nullptr) {				
		return true;
	}
	return false;
}
```

A C++ codeblock with title and line numbers:

``` c++ title="Create a listener model" linenums="1"
template <typename T>
bool ofApp::AddListenerModel(const std::string& _listenerID) {
	brtManager.BeginSetup();
	std::shared_ptr<T>listenerModel = brtManager.CreateListenerModel<T>(_listenerID);
	brtManager.EndSetup();
	if (listenerModel != nullptr) {				
		return true;
	}
	return false;
}
```
