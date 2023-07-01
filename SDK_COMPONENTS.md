# SDK Components

## Input Only components

    ftr.File.Upload(types?: str[],
                title?: str, 
                description?: str,
                min_files?: int, 
                max_files?: int)

    ftr.Text.In(default_text?: str or str[], # if str and num_inputs > 1, default all textboxes to the passed str,
                title?: str,
                description?: str,
                num_inputs?: int, # number of text boxes to render. Defaults to 1
                max_chars?: int or int[])

## Potential Interact

    ftr.List.SelectOne(list: str[],
                title?: str,
                description?: str,
                style?: dropdown or radio)

    ftr.List.SelectMulti(list: str[] or Tuple(str, boolean)[],
                title?: str,
                description?: str,)

    ftr.Image.WithSelectOne(images: array{"name": str, "data": np.array} or numpy[],
                lists: str[][],
                title?: str,
                description?: str,
                style: dropdown or radio # probably default to radio for best UI experience)

    ftr.Image.WithSelectMulti(images: array{"name": str, "data": np.array} or numpy[],
                lists: str[][] or Tuple(str, boolean)[][],
                title?: str,
                description?: str)


    ftr.Image.WithText(images: array{"name": str, "data": np.array} or numpy[],
                default_text?: str[], # default_text gives you the placeholder text. Note the signature change from `WithCheckboxes`
                title?: str,
                description?: str,)

    ftr.Document.WithText(documents: array{"name": str, "data": str}[] or str[],
                text?: str[],
                title?: str,
                description?: str,)

## Output Components

    ftr.File.Download(files: array{"name": str, "data": str} or str[],
                output_filenames?: str[], # defaults to `name` from items if not provided or an enumeration if `name` is not provided
                title?: str,
                description?: str)

    ftr.Document.View(documents: {"name": str, "data": str} or str[],
                output_text?: str[],
                title?: str,
                description?: str)

    ftr.Image.View(images: {"name": str, "data": numpy} or numpy[],
                output_text?: str[],
                title?: str,
                description?: str)

    ftr.Text.View(text: str,
                title?: str)
