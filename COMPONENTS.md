# Overview

This file serves as (potentially temporary) documentation for our [SDK components repo](https://github.com/feather-ai/feather-web-components). Our components are strongly typed via typescript - but some props for components are optional. Here, we'll alphabetically outline the components we currently have, alongside with the interface they expect to receive. Where relevant, there are comments about the current and proposed implementations.

### [CheckboxList](https://github.com/feather-ai/feather-web-components/blob/main/src/components/CheckboxList.tsx):

```javascript
type CheckboxListProps = {
  title?: string,
  options: string[],
  optionValues?: boolean[]; // if provided, needs to be the same length as options. Defaults to false if not provided
};

// Example inputs:
const title = "Model Config"
const options = ["Disable Normalization", "Use Latent Variables"]
const optionValues = [true, false]
const args = {title, option, optionValues}

<CheckboxList {...args} />
```

Proposed Python API:

```python
title = "Model Config"
options = ["Disable Normalization", "Use Latent Variables"]
optionValues = [True, False] # Note the discrepancy between python bools and JS bools
ftr.C.CheckboxList(title=title, option=options, optionValues=optionValues)

# Let's say the user selects both options.
# Proposed return:
{"Disable Normalization": True, "Use Latent Variables": True}
```

Comments:

- If we're returning an object, does it make sense to have the `options` input to be an object too?
  - If so, this would require the user to explicitly provide `optionValues` in the input

### [Dropdown](https://github.com/feather-ai/feather-web-components/blob/main/src/components/Dropdown.tsx):

```javascript
type DropdownProps = {
  title?: string;
  placeholderValue?: string;
  options: string[];
};

// Example inputs:
const title = "My Dropdown"
const options: ["Option 1", "Option 2"]
const placeholderValue: "Click me!",
const args = {title, option, placeholderValue}

<Dropdown {...args} />
```

Proposed Python API:

```python
title = "My Dropdown"
options: ["Option 1", "Option 2"]
placeholderValue: "Click me!",
ftr.C.Dropdown(title=title, option=options, placeholderValue=placeholderValue)

# Let's say the user selects the second option.
# Proposed return:
"Option 2"
```

### [FileUpload](https://github.com/feather-ai/feather-web-components/blob/main/src/components/FileUpload.tsx):

```javascript
type FileUploadProps = {
  types?: string; // Accepts MIME types: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#unique_file_type_specifiers
};

// Example inputs:
const types = "image/*,.docx"
const args = {types}

<FileUpload {...args} />
```

Proposed Python API:

```python
types = ["images", ".txt", ".docx", "videos"] # perhaps we should allow string or string[] as input.
ftr.C.FileUpload(types=types)

# Proposed return:
{"myImage.jpg": <numpy array>, "myText.txt": <string>, "myDocument1.docx": <base64>, "myVideo.mp4": <base64>}
```

Comments:

- For the proposed Python API, regarding the types input, should we allow a `string` and `string[]` as input?
  - if so, `string` would probably serve the case where there's only one filetype accepted (e.g. "images" or ".docx"). It's not worth the overhead to accept and convert string of MIME types (because we've abstracted `images/*` to `images`)
- How do we return the uploaded files? With some file types it's obvious (e.g. images, .txt etc.). With others, it's a bit too much overhead to decode the files for them.. is base64 encoding them a worthy compromise?

### [ImageViewer](https://github.com/feather-ai/feather-web-components/blob/main/src/components/ImageViewer.tsx):

```javascript
type IndividualImageType = {
  original: string;
  thumbnail: string;
};

type ImageViewerType = {
  images: IndividualImageType[];
};

// Example inputs:
const images = [
  {
    original: "https://picsum.photos/id/1018/1000/600/",
    thumbnail: "https://picsum.photos/id/1018/250/150/",
  },
  {
    original: "https://picsum.photos/id/1015/1000/600/",
    thumbnail: "https://picsum.photos/id/1015/250/150/",
  },
  {
    original: "https://picsum.photos/id/1019/1000/600/",
    thumbnail: "https://picsum.photos/id/1019/250/150/",
  },
];
const args = {images}

<ImageViewer {...args} />
```

Proposed Python API:

```python
images = [<3d-NUMPY-ARRAY>, <3d-NUMPY-ARRAY>, <3d-NUMPY-ARRAY>]
ftr.C.ImageViewer(images=images) # we'd internally create a thumbnail from the arrays

# No return - this is a view only component
```

Comments:

- I built this component with a gallery view in mind (i.e. renders multiple images from an image generator). But writing this documentation made me realise some people might like to use a single image viewer to display an image onto the model page.
- TODO: Modify this component to accept a single image
- We internally need to create thumbnails for images in the gallery use-case
- TODO: Currently accepts URLs for images. Need to check whether we can send in array data or a b64 string. Otherwise we'll need to use Cloudinary or Cloudfront to store the images

### [ObjectViewer](https://github.com/feather-ai/feather-web-components/blob/main/src/components/ObjectViewer.tsx):

```javascript
type ObjectViewerProps = {
  json: string;
};

// Example inputs
const json = `{
  "House Number": 123,
  "House Name": "",
  "Building Name": "FlatHouse Place",
  "Road Name": "Residence Road",
  City: "London",
  County: "London",
  Country: "UK",
  Postcode: "F4K3 P0C0",
}`;
const args = {json}

<ObjectViewer {...args} />
```

Proposed Python API:

```python
addressObject = {
  "House Number": 123,
  "House Name": "",
  "Building Name": "FlatHouse Place",
  "Road Name": "Residence Road",
  "City": "London",
  "County": "London",
  "Country": "UK",
  "Postcode": "F4K3 P0C0",
} # This'll be a python dictionary type

ftr.C.ObjectViewer(dictionary=addressObject)

# No return - this is a view only component
```

Comments:

- TODO: Change `json` prop to `dictionary`
- TODO: Add support for multiple dictionaries

### [ScrollableImageSelector](https://github.com/feather-ai/feather-web-components/blob/main/src/components/ScrollableImageSelector.tsx):

```javascript
export interface ScrollableImageSelectorProps {
  imageUrls: string[];
  handleNewImageSelect: (i: number) => void;
}

// Example inputs
const imageUrls = [
  "https://images1.westend61.de/0001012337pw/alsatian-dog-catching-frisbee-ISF18166.jpg",
  "https://pbs.twimg.com/media/DCnBDANUIAAdMEQ?format=jpg&name=large",
  "https://www.thedailymeal.com/sites/default/files/story/eating%20pizza-iStock-ThinkstockPhotos-470336604.jpg",
];

const handleNewImageSelect = (i: number) => {
  console.log("You clicked on number", i);
};
const args = {imageUrls, handleNewImageSelect}

<ScrollableImageSelector {...args} />
```

Proposed Python API:

```python
images = [<3d-NUMPY-ARRAY>, <3d-NUMPY-ARRAY>, <3d-NUMPY-ARRAY>]
texts = ["This is image1", "This is image2", "This is image3"]
ftr.C.ScrollableImageSelector(images=images, texts=texts)

# No return - this is a view only component
```

Comments:

- TODO: Change the name of ScrollableImageSelector (proposed: ImagesWithText)
- TODO: Change required props from `imageUrls, handleNewImageSelect` to `images, texts`

### [ScrollableImageSelectorWithCheckboxObjectList](https://github.com/feather-ai/feather-web-components/blob/main/src/components/ScrollableImageSelectorWithCheckboxObjectList.tsx):

```javascript
export type ScrollableImageSelectorWithCheckboxObjectListProps = {
  imageUrls: string[];
  objList: string[][]; // List of lists. Each nested list contains the detected objects (via an object detection model) in an image
};

// Example inputs
const imageUrls = [
  "https://images1.westend61.de/0001012337pw/alsatian-dog-catching-frisbee-ISF18166.jpg",
  "https://pbs.twimg.com/media/DCnBDANUIAAdMEQ?format=jpg&name=large",
  "https://www.thedailymeal.com/sites/default/files/story/eating%20pizza-iStock-ThinkstockPhotos-470336604.jpg",
];
const objList = [
  ["Dog", "Frisbee", "Person", "Tree"],
  ["Faucet", "Cat", "Bottle", "Sponge", "Gloves", "Sink"],
  ["People", "Box", "Pizza", "Sofa"],
];
const args = {imageUrls, objList}

<ScrollableImageSelectorWithCheckboxObjectList {...args} />
```

Proposed Python API:

```python
images = [<3d-NUMPY-ARRAY>, <3d-NUMPY-ARRAY>, <3d-NUMPY-ARRAY>]
objects = [
  ["Dog", "Frisbee", "Person", "Tree"],
  ["Faucet", "Cat", "Bottle", "Sponge", "Gloves", "Sink"],
  ["People", "Box", "Pizza", "Sofa"],
];
ftr.C.ScrollableImageSelectorWithCheckboxObjectList(images=images, objects=objects)

# No return - this is a view only component
```

Comments:

- TODO: Change the name of ScrollableImageSelectorWithCheckboxObjectList (proposed: ImagesWithSelectableObjects)
- TODO: Change required props from `objList` to `objects`

### [Textbox](https://github.com/feather-ai/feather-web-components/blob/main/src/components/Textbox.tsx):

```javascript
type TextboxProps = {
  title: string; // takes on the "label" attribute in the html text field
  defaultValue: string;
};

// Example inputs:
const title = "Enter Name"
const defaultValue: "Your Name...",
const args = {title, defaultValue}

<Textbox {...args} />
```

Proposed Python API:

```python
title = "Enter Name"
defaultValue: "Your Name...",
ftr.C.Textbox(title=title, defaultValue=defaultValue)

# Returns the entered text... obviously
# Proposed return:
"Nihir"
```

Comments:

- TODO: Think about how to support multiline inputs. (Proposed: Always render a multiline field and have it auto expand as the consumer types)
- Should defaultValue be optional?
