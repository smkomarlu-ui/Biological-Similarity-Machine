# Protein Molecular Weight Calculator 🧬

A Python script that calculates the molecular weight (in Daltons) of a protein from its amino acid sequence, using average residue masses and accounting for water loss during peptide bond formation.

## How It Works

Given a sequence of single-letter amino acid codes, the script:

1. Looks up the average molecular weight of each amino acid residue (in Daltons)
2. Sums the weights across the sequence
3. Subtracts the mass of water lost for each peptide bond formed (since each bond releases one H₂O molecule during synthesis)

The final formula is:

```
total_mass = (sum of residue weights) - (number of peptide bonds × 18.015)
```

where `peptide_bonds = length(sequence) - 1`.

## Usage

Run the script and enter a protein sequence when prompted:

```bash
python protein_calculator.py
```

```
Enter a protein sequence (using single-letter amino acid codes): MKVLA
The total molecular weight of the protein sequence is: 549.68 Daltons
```

Input is automatically converted to uppercase, so lowercase letters work too.

## Supported Amino Acids

The calculator recognizes the 20 standard amino acids by their single-letter codes:

| Code | Amino Acid | Weight (Da) | Code | Amino Acid | Weight (Da) |
|------|-----------|-------------|------|-----------|-------------|
| A | Alanine | 89.09 | M | Methionine | 149.21 |
| R | Arginine | 174.20 | F | Phenylalanine | 165.19 |
| N | Asparagine | 132.12 | P | Proline | 115.13 |
| D | Aspartate | 133.10 | S | Serine | 105.09 |
| C | Cysteine | 121.16 | T | Threonine | 119.12 |
| Q | Glutamine | 146.15 | W | Tryptophan | 204.23 |
| E | Glutamate | 147.13 | Y | Tyrosine | 181.19 |
| G | Glycine | 75.07 | V | Valine | 117.15 |
| H | Histidine | 155.16 | K | Lysine | 146.19 |
| I | Isoleucine | 131.18 | L | Leucine | 131.18 |

## Error Handling

- **Empty input** — if the sequence is empty, the script prints a message and returns without calculating.
- **Invalid amino acid codes** — if a character in the sequence doesn't match one of the 20 standard codes, the script prints the invalid character and stops processing (no weight is returned).

## Requirements

- Python 3.6+ (uses f-strings)
- No external dependencies — standard library only

## Installation

```bash
git clone https://github.com/yourusername/protein-calculator.git
cd protein-calculator
python protein_calculator.py
```

## Example

```
Enter a protein sequence (using single-letter amino acid codes): acdefghiklmnpqrstvwy
The total molecular weight of the protein sequence is: 2394.99 Daltons
```

## Limitations / Future Improvements

- Currently interactive-only (no command-line arguments or file input support)
- Does not support non-standard/modified amino acids or post-translational modifications
- Could be extended to accept sequences from FASTA files

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
