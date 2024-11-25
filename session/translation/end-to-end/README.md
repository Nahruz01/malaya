# HuggingFace T5

## how to primeintellect

```bash
cd /workspace
apt update
apt install vim screen -y
pip3 install huggingface-hub wandb mosaicml-streaming evaluate
huggingface-cli login
wandb wandb login
pip3 install notebook==6.5.6
screen -dmS jupyter_session bash -c "jupyter notebook --NotebookApp.token='' --no-browser --allow-root --notebook-dir='/workspace'"
python -c "
from huggingface_hub import snapshot_download
snapshot_download(repo_id='mesolitica/malaysian-translation-v2-multipack-2048-post', repo_type='dataset', local_dir = './malaysian-translation-v2-multipack-2048-post')
"
pip3 install git+https://github.com/mesolitica/t5-sdpa-multipack
bash nanot5-small-multipack-post.sh
```

1. Run prepare dataset, [prepare-data-v2.ipynb](prepare-data-v2.ipynb).

2. Run training script,

Original script, https://github.com/huggingface/transformers/blob/v4.21.2/examples/pytorch/translation/run_translation.py

BASE model,
```
CUDA_VISIBLE_DEVICES='0' \
WANDB_DISABLED=true \
python3 run_t5_v2.py \
--model_name_or_path mesolitica/nanot5-base-malaysian-cased \
--num_train_epochs 2 \
--logging_steps 100 \
--eval_steps 1000000000 \
--save_steps 10000 \
--evaluation_strategy steps \
--save_total_limit 3 \
--do_train \
--do_eval \
--bf16 \
--source_lang src \
--target_lang tgt \
--train_file shuffled-train.json \
--validation_file test.json \
--output_dir nanot5-base-malaysian-cased \
--per_device_train_batch_size=5 \
--per_device_eval_batch_size=4 \
--predict_with_generate \
--max_source_length 2048 \
--max_target_length 2048 \
--learning_rate 2e-4 \
--gradient_checkpointing true
```

SMALL model,
```
CUDA_VISIBLE_DEVICES='1' \
WANDB_DISABLED=true \
python3 run_t5_v2.py \
--model_name_or_path mesolitica/nanot5-small-malaysian-cased \
--num_train_epochs 2 \
--logging_steps 100 \
--eval_steps 1000000000 \
--save_steps 10000 \
--evaluation_strategy steps \
--save_total_limit 3 \
--do_train \
--do_eval \
--bf16 \
--source_lang src \
--target_lang tgt \
--train_file shuffled-train.json \
--validation_file test.json \
--output_dir nanot5-small-malaysian-cased \
--per_device_train_batch_size=6 \
--per_device_eval_batch_size=4 \
--predict_with_generate \
--max_source_length 2048 \
--max_target_length 2048 \
--learning_rate 2e-4 \
--gradient_checkpointing true
```

TINY model,
```
CUDA_VISIBLE_DEVICES='1' \
WANDB_DISABLED=true \
python3 run_t5_v2.py \
--model_name_or_path mesolitica/nanot5-tiny-malaysian-cased \
--num_train_epochs 2 \
--logging_steps 100 \
--eval_steps 1000000000 \
--save_steps 10000 \
--evaluation_strategy steps \
--save_total_limit 3 \
--do_train \
--do_eval \
--bf16 \
--source_lang src \
--target_lang tgt \
--train_file shuffled-train.json \
--validation_file test.json \
--output_dir nanot5-tiny-malaysian-cased \
--per_device_train_batch_size=6 \
--per_device_eval_batch_size=4 \
--predict_with_generate \
--max_source_length 2048 \
--max_target_length 2048 \
--learning_rate 2e-4 \
--gradient_checkpointing true
```