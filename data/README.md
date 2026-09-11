# Dataset

The dataset is **not** committed to this repository (it is ignored via `.gitignore`).
Supply your own and place it here as:

```
data/training_data.pickle
```

## Expected format

`training_data.pickle` is a Python pickle of a **list of `(image, label)` pairs**:

```python
import pickle
training_data = pickle.load(open("data/training_data.pickle", "rb"))
# training_data -> [(image, label), (image, label), ...]
```

- **image** — an RGB array of shape `512 × 512 × 3` (compatible with
  `torchvision.transforms.ToPILImage`), i.e. `uint8` H×W×C.
- **label** — an integer in `{1, 2, 3}`:

  | Label | Class       |
  |-------|-------------|
  | 1     | Meningioma  |
  | 2     | Glioma      |
  | 3     | Pituitary   |

The notebook maps labels to a 0-based index internally (`label - 1`) for training.

## Notes

- The pipeline applies **8× augmentation** per image (original + 7 rotations), so the
  effective dataset size is 8× the number of raw samples.
- A common public source for this task is the **Figshare brain-tumor dataset**
  (Cheng et al.), which contains T1-weighted contrast-enhanced MRI images across these
  three tumour types. Convert it into the pickle format above before use.
